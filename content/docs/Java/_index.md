---
# Title, summary, and page position.
linktitle: 🧙‍♂️ 必知必会
summary: Review
weight: 1
icon: java
icon_pack: fab

# Page metadata.
title: 复习回顾
date: "2021-01-01T00:00:00Z"
type: book  # Do not modify.
---

## 集合

- [x] Java 容器有哪些？ 哪些是同步容器？ 哪些是并发容器？
- [x] HashMap 与 ConcurrentHashMap 的实现原理是怎样的？ConcurrentHashMap 是如何保证线程安全的？
- [x] 集合类中的 List 和 Map 的线程安全版本是什么，如何保证线程安全的？
- [ ] 简述 HashMap 和 TreeMap 的实现原理以及常见操作的时间复杂度
- [ ] 简述 ArrayList 与 LinkedList 的底层实现以及常见操作的时间复杂度
- [ ] hashmap 和 hashtable 的区别是什么？
- [ ] HashMap 实现原理，为什么使用红黑树？
- [ ] hashMap 1.7 / 1.8 的实现区别


## 并发

- [x] volatile 关键字解决了什么问题，它的实现原理是什么？
- [x] synchronized 关键字底层是如何实现的？它与 Lock 相比优缺点分别是什么？
- [x] 简述 Synchronized，Volatile，可重入锁（ReetrantLock） 的不同使用场景及优缺点？
- [x] ThreadLocal 实现原理是什么？
- [x] Java 常见锁有哪些？ReetrantLock 是怎么实现的？
- [ ] Java 线程和操作系统的线程是怎么对应的？Java线程是怎样进行调度的?
- [x] Java 中 sleep() 与 wait() 的区别
- [x] Java 线程池里的 arrayblockingqueue 与 linkedblockingqueue 的使用场景和区别
- [x] 线程池是如何实现的？简述线程池的任务策略
- [x] Java 线程间有多少通信方式？
- [x] CAS 实现原理是什么？
- [x] 谈谈对AQS的理解
- [ ] 简述乐观锁以及悲观锁的区别以及使用场景
- [ ] 什么情况下会发生死锁，如何解决死锁？

## JVM

- [ ] Java 中垃圾回收机制中如何判断对象需要回收？常见的 GC 回收算法有哪些？
- [ ] 简述 JVM 的内存模型 JVM 内存是如何对应到操作系统内存的？
- [ ] JVM 中内存模型是怎样的，简述新生代与老年代的区别？
- [ ] Java 类的加载流程是怎样的？什么是双亲委派机制？
- [ ] JVM 是怎么去调优的？简述过程和调优的结果