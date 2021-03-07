---
title: Java 集合
linktitle: Java 集合
type: book
date: "2021-01-04T00:00:00+01:00"

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---


### ConcurrentHashMap

ConcurrentHashMap 底层是基于 数组 + 链表 组成的，不过在 jdk1.7 和 1.8 中具体实现稍有不同。

ConcurrentHashMap在 1.7 的时候，是由Segment 数组、HashEntry 组成，和 HashMap 一样，仍然是数组加链表。

Segment 是 ConcurrentHashMap 的一个内部类，主要的组成如下：

```java
static final class Segment<K,V> extends ReentrantLock implements Serializable {    
    private static final long serialVersionUID = 2249069246763182397L;    
    // 和 HashMap 中的 HashEntry 作用一样，真正存放数据的桶    
    transient volatile HashEntry<K,V>[] table;    
    transient int count;        
    transient int modCount;        
    // 大小    
    transient int threshold;        
    // 负载因子    
    final float loadFactor;
}
```

HashEntry跟HashMap差不多的，但是不同点是，他使用volatile去修饰了他的Value还有下一个节点next。

原理上来说，ConcurrentHashMap 采用了分段锁技术，其中 Segment 继承于 ReentrantLock。不会像 HashTable 那样不管是 put 还是 get 操作都需要做同步处理，理论上 ConcurrentHashMap 支持 CurrencyLevel (Segment 数组数量)的线程并发。

每当一个线程占用锁访问一个 Segment 时，不会影响到其他的 Segment。就是说如果容量大小是16他的并发度就是16，可以同时允许16个线程操作16个Segment而且还是线程安全的。

添加元素时，先定位到 Segment ，然后再进行put操作。 首先第一步的时候会尝试获取锁，如果获取失败肯定就有其他线程存在竞争，则利用 scanAndLockForPut() 自旋获取锁。如果重试的次数达到了 MAX_SCAN_RETRIES 则改为阻塞锁获取，保证能获取成功。

```java
public V put(K key, V value) {    
Segment<K,V> s;    
if (value == null)        
    throw new NullPointerException(); //这就是为啥他不可以put null值的原因    
int hash = hash(key);    
int j = (hash >>> segmentShift) & segmentMask;    
if ((s = (Segment<K,V>)UNSAFE.getObject                   
    (segments, (j << SSHIFT) + SBASE)) == null)         
    s = ensureSegment(j);    
return s.put(key, hash, value, false);
}

final V put(K key, int hash, V value, boolean onlyIfAbsent) {          
       // 将当前 Segment 中的 table 通过 key 的 hashcode 定位到 HashEntry            
    HashEntry<K,V> node = tryLock() ? null :                
    scanAndLockForPut(key, hash, value);            
    V oldValue;            
    try {                
        HashEntry<K,V>[] tab = table;                
        int index = (tab.length - 1) & hash;             
        HashEntry<K,V> first = entryAt(tab, index);                
        for (HashEntry<K,V> e = first;;) {                    
            if (e != null) {                        
            K k; 
            // 遍历该 HashEntry，如果不为空则判断传入的 key 和当前遍历的 key 是否相等，相等则覆盖旧的 value。                        
            if ((k = e.key) == key ||                            
                (e.hash == hash && key.equals(k))) {                            
                oldValue = e.value;                            
                if (!onlyIfAbsent) {                                
                    e.value = value;                                
                    ++modCount;                            
                }                            
                break;                        
            }                        
            e = e.next;                    
            } else {                 
                // 不为空则需要新建一个 HashEntry 并加入到 Segment 中，同时会先判断是否需要扩容。                        
                if (node != null)                            
                    node.setNext(first);                        
                else                            
                    node = new HashEntry<K,V>(hash, key, value, first);                        
                int c = count + 1;                        
                if (c > threshold && tab.length < MAXIMUM_CAPACITY)                            
                    rehash(node);                        
                else                            
                    setEntryAt(tab, index, node);                        

                ++modCount;                        
                count = c;                        
                oldValue = null;                        
                break;                    
                }                
            }            
        } 
    finally {               
    //释放锁                
        unlock();            
    }            
    return oldValue;        
}
```

get 逻辑比较简单，只需要将 Key 通过 Hash 之后定位到具体的 Segment ，再通过一次 Hash 定位到具体的元素上。由于 HashEntry 中的 value 属性是用 volatile 关键词修饰的，保证了内存可见性，所以每次获取时都是最新值。 ConcurrentHashMap 的 get 方法是非常高效的，因为整个过程都不需要加锁。

1.8版本的 ConcurrentHashMap 抛弃了 Segment 分段锁的实现，采用了 `CAS + Synchronized` 来保证并发安全。也把之前的HashEntry改成了Node，但是作用不变，把值和next采用了volatile去修饰，保证了可见性，并且也引入了红黑树，在链表大于一定值的时候会转换（默认是8）。

```java
static class Node<K,V> implements Map.Entry<K,V> {
    final int hash;
    final K key;
    volatile V val;
    volatile Node<K,V> next;
}
```

ConcurrentHashMap在进行put操作的还是比较复杂的，大致可以分为以下步骤：

- 根据 key 计算出 hashcode 。
- 判断是否需要进行初始化。
- 根据当前 key 定位出的 Node，如果为空表示当前位置可以写入数据，利用 CAS 尝试写入，失败则自旋保证成功。
- 如果当前位置的 hashcode == MOVED == -1,则需要进行扩容。
- 如果都不满足，则利用 synchronized 锁写入数据。
- 如果数量大于 TREEIFY_THRESHOLD 则要转换为红黑树。

```java
public V put(K key, V value) {
    return putVal(key, value, false);
}

final V putVal(K key, V value, boolean onlyIfAbsent) {
    if (key == null || value == null) throw new NullPointerException();
    // 得到 hash 值
    int hash = spread(key.hashCode());
    // 用于记录相应链表的长度
    int binCount = 0;
    for (Node<K, V>[] tab = table; ; ) {
        Node<K, V> f;
        int n, i, fh;
        // 如果数组"空"，进行数组初始化
        if (tab == null || (n = tab.length) == 0)
            // 初始化数组，后面会详细介绍
            tab = initTable();

            // 找该 hash 值对应的数组下标，得到第一个节点 f
        else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
            // 如果数组该位置为空，
            //    用一次 CAS 操作将这个新值放入其中即可，这个 put 操作差不多就结束了，可以拉到最后面了
            //          如果 CAS 失败，那就是有并发操作，进到下一个循环就好了
            if (casTabAt(tab, i, null,
                    new Node<K, V>(hash, key, value, null)))
                break;                   // no lock when adding to empty bin
        }
        // hash 居然可以等于 MOVED，这个需要到后面才能看明白，不过从名字上也能猜到，肯定是因为在扩容
        else if ((fh = f.hash) == MOVED)
            // 帮助数据迁移，这个等到看完数据迁移部分的介绍后，再理解这个就很简单了
            tab = helpTransfer(tab, f);

        else { // 到这里就是说，f 是该位置的头结点，而且不为空

            V oldVal = null;
            // 获取数组该位置的头结点的监视器锁
            synchronized (f) {
                if (tabAt(tab, i) == f) {
                    if (fh >= 0) { // 头结点的 hash 值大于 0，说明是链表
                        // 用于累加，记录链表的长度
                        binCount = 1;
                        // 遍历链表
                        for (Node<K, V> e = f; ; ++binCount) {
                            K ek;
                            // 如果发现了"相等"的 key，判断是否要进行值覆盖，然后也就可以 break 了
                            if (e.hash == hash &&
                                    ((ek = e.key) == key ||
                                            (ek != null && key.equals(ek)))) {
                                oldVal = e.val;
                                if (!onlyIfAbsent)
                                    e.val = value;
                                break;
                            }
                            // 到了链表的最末端，将这个新值放到链表的最后面
                            Node<K, V> pred = e;
                            if ((e = e.next) == null) {
                                pred.next = new Node<K, V>(hash, key,
                                        value, null);
                                break;
                            }
                        }
                    } else if (f instanceof TreeBin) { // 红黑树
                        Node<K, V> p;
                        binCount = 2;
                        // 调用红黑树的插值方法插入新节点
                        if ((p = ((TreeBin<K, V>) f).putTreeVal(hash, key,
                                value)) != null) {
                            oldVal = p.val;
                            if (!onlyIfAbsent)
                                p.val = value;
                        }
                    }
                }
            }
            // binCount != 0 说明上面在做链表操作
            if (binCount != 0) {
                // 判断是否要将链表转换为红黑树，临界值和 HashMap 一样，也是 8
                if (binCount >= TREEIFY_THRESHOLD)
                    // 这个方法和 HashMap 中稍微有一点点不同，那就是它不是一定会进行红黑树转换，
                    // 如果当前数组的长度小于 64，那么会选择进行数组扩容，而不是转换为红黑树
                    //    具体源码我们就不看了，扩容部分后面说
                    treeifyBin(tab, i);
                if (oldVal != null)
                    return oldVal;
                break;
            }
        }
    }
    //
    addCount(1L, binCount);
    return null;
}
```

初始化方法中的并发问题是通过对 sizeCtl 进行一个 CAS 操作来控制的。

```java
private final Node<K,V>[] initTable() {
    Node<K,V>[] tab; int sc;
    while ((tab = table) == null || tab.length == 0) {
        // 初始化的"功劳"被其他线程"抢去"了
        if ((sc = sizeCtl) < 0)
            Thread.yield(); // lost initialization race; just spin
        // CAS 一下，将 sizeCtl 设置为 -1，代表抢到了锁
        else if (U.compareAndSwapInt(this, SIZECTL, sc, -1)) {
            try {
                if ((tab = table) == null || tab.length == 0) {
                    // DEFAULT_CAPACITY 默认初始容量是 16
                    int n = (sc > 0) ? sc : DEFAULT_CAPACITY;
                    // 初始化数组，长度为 16 或初始化时提供的长度
                    Node<K,V>[] nt = (Node<K,V>[])new Node<?,?>[n];
                    // 将这个数组赋值给 table，table 是 volatile 的
                    table = tab = nt;
                    // 如果 n 为 16 的话，那么这里 sc = 12
                    // 其实就是 0.75 * n
                    sc = n - (n >>> 2);
                }
            } finally {
                // 设置 sizeCtl 为 sc，我们就当是 12 吧
                sizeCtl = sc;
            }
            break;
        }
    }
    return tab;
}
```




