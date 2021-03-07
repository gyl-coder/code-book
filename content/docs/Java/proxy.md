---
title: java proxy
linktitle: proxy
type: book
date: "2021-01-01T00:00:00+01:00"

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---

## Java 代理

代理模式是一种比较好理解的设计模式。

简单来说就是：我们使用代理对象来代替对真实对象(real object)的访问，这样就可以在不修改原目标对象的前提下，提供额外的功能操作，达到扩展目标对象功能的目的。

> 生活中的代购、租房中介、售票黄牛、婚介、经纪人、快递、事务代理、非侵入式日志监听等，都是代理模式的实际体现。
> 使用代理模式主要有两个目的：
> 1. 保护目标对象。
> 2. 增强目标对象（比如说在目标对象的某个方法执行前后你可以增加一些自定义的操作）

代理模式中一般包含三种角色：

- `抽象主题角色（Subject）`：抽象主题类的主要职责是声明真实主题与代理的共同接口方法，该类可以是接口也可以是抽象类
- `真实主题角色（RealSubject）`：该类也被称为被代理类，该类定义了代理所表示的真实对象，是负责执行系统真正的逻辑业务对象
- `代理主题角色（Proxy）`：也被称为代理类，其内部持有 RealSubject 的引用，因此具备完全的对 RealSubject的代理权。客户端调用代理对象的方法，同时也调用被代理对象的方法，但是会在代理对 象前后增加一些处理代码。

在代码中，一般代理会被理解为`代码增强`，实际上就是在原代码逻辑前后增加一些代码逻辑，而使调用者无感知。代理模式属于结构型模式，分为: `静态代理` 和 `动态代理`。

### 静态代理

静态代理需要为每个被代理的对象手动创建一个代理类；而动态代理则时在运行时通过某种机制来动态生成，不需要手动创建代理类。

### JDK 动态代理

jdk动态代理模式是利用java中的**反射技术，在运行时动态创建代理类**。

基于动态jdk涉及到两个核心的类：`Proxy类` 和 `InvocationHandler接口`

JDK动态代理的写法比较固定，需要先定义一个接口和接口的实现类，然后再定义一个实现了InvocationHandler接口的实现类。最后调用Proxy类的newInstance()方法即可。示例代码如下：

```java
// 先定义一个接口：UserService，接口中有两个方法
public interface UserService {
    int insert();
    String query();
}

// 再定义一个UserService接口的实现类：UserServiceImpl
public class UserServiceImpl implements UserService{

    @Override
    public int insert() {
        System.out.println("insert");
        return 0;
    }

    @Override
    public String query() {
        System.out.println("query");
        return null;
    }
}

// 再定义一个InvocationHandler接口的实现类：UserServiceInvocationHandler。在自定义的InvocationHandler中，定义了一个属性：target，定义这个属性的目的是为了在InvocationHandler中持有对目标对象的引用，target属性的初始化是在构造器中进行初始化的。
public class UserServiceInvocationHandler implements InvocationHandler {

    // 持有目标对象
    private Object target;

    public UserServiceInvocationHandler(Object target){
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("invocation handler");
        // 通过反射调用目标对象的方法
        return method.invoke(target,args);
    }
}

// 通过Proxy.newProxyInstance()方法创建代理对象
public class MainApplication {

    public static void main(String[] args) {
        // 指明一个类加载器，要操作class文件，怎么少得了类加载器呢
        ClassLoader classLoader = MainApplication.class.getClassLoader();
        // 为代理对象指定要是实现哪些接口，这里我们要为UserServiceImpl这个目标对象创建动态代理，所以需要为代理对象指定实现UserService接口
        Class[] classes = new Class[]{UserService.class};
        // 初始化一个InvocationHandler，并初始化InvocationHandler中的目标对象
        InvocationHandler invocationHandler = new UserServiceInvocationHandler(new UserServiceImpl());
        // 创建动态代理
        UserService userService = (UserService) Proxy.newProxyInstance(classLoader, classes, invocationHandler);
        // 执行代理对象的方法，通过观察控制台的结果，判断我们是否对目标对象(UserServiceImpl)的方法进行了增强
        userService.insert();
    }
}

// 控制台打印结果如下，从打印结果来看，已经完成了对目标对象UserServiceImpl的代理（增强）。
Output:

invocation handler
insert
```

jdk实现动态代理可以分为以下几个步骤：

1. 先检查委托类是否实现了相应接口，保证被访问方法在接口中也要有定义
2. 创建一个实现InvocationHandler接口的类
3. 在类中定义一个被代理对象的成员属性，为了扩展方便可以直接使用Object类，也可以根据需求定义相应的接口
4. 在invoke方法中实现对委托对象的调用，根据需求对方法进行增强
5. 使用Proxy.newProxyInstance(...)方法创建代理对象，并提供要给获取代理对象的方法

下面我们通过一些手段，来看下jdk动态生成的代理类长什么样子。

**手段一：**

自定义的一个小工具类，可以将动态生成的代理类保存到本地

```java
/**
 * 将生成的代理类保存为.class文件的工具类
 */
public class ProxyUtils {
    /**
     * 将代理类保存到指定路径
     *
     * @param path           保存到的路径
     * @param proxyClassName 代理类的Class名称
     * @param interfaces     代理类接口
     * @return
     */
    public static boolean saveProxyClass(String path, String proxyClassName, Class[] interfaces){
        if (proxyClassName == null || path == null) {
            return false;
        }
        // 获取文件字节码，然后输出到目标文件中
        byte[] classFile = ProxyGenerator.generateProxyClass(proxyClassName, interfaces);
        try (FileOutputStream out = new FileOutputStream(path)) {
            out.write(classFile);
            out.flush();
        } catch (IOException e) {
            e.printStackTrace();
            return false;
        }
        return true;
    }
}
```

**手段二：**

在代码中添加一行代码：`System.getProperties().put("sun.misc.ProxyGenerator.saveGeneratedFiles", "true")`，就能实现将程序运行过程中产生的动态代理对象的class文件写入到磁盘。如下示例：

- 运行程序，最终发现在项目的根目录下出现了一个包：com.sun.proxy。包下有一个文件`$Proxy0.class`。在idea打开，发现就是所产生代理类的源代码。

```java
public class MainApplication {

    public static void main(String[] args) {
        // 让代理对象的class文件写入到磁盘
        System.getProperties().put("sun.misc.ProxyGenerator.saveGeneratedFiles", "true");
        // 指明一个类加载器，要操作class文件，怎么少得了类加载器呢
        ClassLoader classLoader = MainApplication.class.getClassLoader();
        // 为代理对象指定要是实现哪些接口，这里我们要为UserServiceImpl这个目标对象创建动态代理，所以需要为代理对象指定实现UserService接口
        Class[] classes = new Class[]{UserService.class};
        // 初始化一个InvocationHandler，并初始化InvocationHandler中的目标对象
        InvocationHandler invocationHandler = new UserServiceInvocationHandler(new UserServiceImpl());
        // 创建动态代理
        UserService userService = (UserService) Proxy.newProxyInstance(classLoader, classes, invocationHandler);
        // 执行代理对象的方法，通过观察控制台的结果，判断我们是否对目标对象(UserServiceImpl)的方法进行了增强
        userService.insert();
    }
}
```

通过上面两种方法获取到代理对象的源代码如下：

```java
package com.sun.proxy;

import com.tiantang.study.UserService;
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
import java.lang.reflect.UndeclaredThrowableException;

public final class $Proxy0 extends Proxy implements UserService {
    private static Method m1;
    private static Method m3;
    private static Method m4;
    private static Method m2;
    private static Method m0;

    public $Proxy0(InvocationHandler var1) throws  {
        super(var1);
    }

    public final boolean equals(Object var1) throws  {
        try {
            return (Boolean)super.h.invoke(this, m1, new Object[]{var1});
        } catch (RuntimeException | Error var3) {
            throw var3;
        } catch (Throwable var4) {
            throw new UndeclaredThrowableException(var4);
        }
    }

    public final int insert() throws  {
        try {
            return (Integer)super.h.invoke(this, m3, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    public final String query() throws  {
        try {
            return (String)super.h.invoke(this, m4, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    public final String toString() throws  {
        try {
            return (String)super.h.invoke(this, m2, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    public final int hashCode() throws  {
        try {
            return (Integer)super.h.invoke(this, m0, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    static {
        try {
            m1 = Class.forName("java.lang.Object").getMethod("equals", Class.forName("java.lang.Object"));
            m3 = Class.forName("com.tiantang.study.UserService").getMethod("insert");
            m4 = Class.forName("com.tiantang.study.UserService").getMethod("query");
            m2 = Class.forName("java.lang.Object").getMethod("toString");
            m0 = Class.forName("java.lang.Object").getMethod("hashCode");
        } catch (NoSuchMethodException var2) {
            throw new NoSuchMethodError(var2.getMessage());
        } catch (ClassNotFoundException var3) {
            throw new NoClassDefFoundError(var3.getMessage());
        }
    }
}
```

通过源码我们发现，`$Proxy0`类继承了Proxy类，同时实现了UserService接口。到这里就能解释：为什么JDK的动态代理只能基于接口实现，不能基于继承来实现？

**因为Java中不支持多继承，而JDK的动态代理在创建代理对象时，默认让代理对象继承了Proxy类，所以JDK只能通过接口去实现动态代理。**

`$Proxy0`实现了UserService接口，所以重写了接口中的两个方法（`$Proxy0`同时还重写了Object类中的几个方法）。所以当我们调用query()方法时，先是调用到`$Proxy0.query()`方法，在这个方法中，直接调用了super.h.invoke()方法，父类是Proxy，父类中的`h`就是我们定义的InvocationHandler，所以这儿会调用到UserServiceInvocationHandler.invoke()方法。因此当我们通过代理对象去执行目标对象的方法时，会先经过InvocationHandler的invoke()方法，然后在通过反射method.invoke()去调用目标对象的方法，因此每次都会先打印`invocation handler`这句话。

参考文章：https://juejin.cn/post/6844903951976890381#heading-0

### CGLIB 动态代理

cglib动态代理和jdk动态代理类似，也是采用**操作字节码机制**，在运行时生成代理类。cglib 动态代理采取的是**创建目标类的子类的方式**，因为是子类化，我们可以达到近似使用被调用者本身的效果。

> 字节码处理机制: 指得是ASM来转换字节码并生成新的类

注：spring中有完整的cglib相关的依赖，所以以下代码基于spring官方下载的demo中直接进行编写的

```java
/**
 * 1. 订单服务-委托类，不需要再实现接口
 **/
public class OrderService {
    /**
     * 保存订单接口
     */
    public void saveOrder(String orderInfo) throws InterruptedException {
        // 随机休眠，模拟订单保存需要的时间
        Thread.sleep(System.currentTimeMillis() & 100);
        System.out.println("订单：" + orderInfo + "  保存成功");
    }
}

/**
 * cglib动态代理工厂
 *
 * @author cruder
 * @date 2019-11-23 18:36
 **/
public class ProxyFactory implements MethodInterceptor {

    /**
     * 委托对象， 即被代理对象
      */
    private Object target;

    public ProxyFactory(Object target) {
        this.target = target;
    }

    /**
     * 返回一个代理对象
     * @return
     */
    public Object getProxyInstance(){
        // 1. 创建一个工具类
        Enhancer enhancer = new Enhancer();
        // 2. 设置父类
        enhancer.setSuperclass(target.getClass());
        // 3. 设置回调函数
        enhancer.setCallback(this);
        // 4.创建子类对象，即代理对象
        return enhancer.create();
    }

    @Override
    public Object intercept(Object o, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
        long start = System.currentTimeMillis();

        Object result = method.invoke(target, args);

        System.out.println("cglib代理：保存订单用时: " + (System.currentTimeMillis() - start) + "ms");
        return result;
    }
}

/**
 * 使用cglib代理类来保存订单
 **/
public class Client {
    public static void main(String[] args) throws InterruptedException {
        // 1. 创建委托对象
        OrderService orderService = new OrderService();
        // 2. 获取代理对象
        OrderService orderServiceProxy = (OrderService) new ProxyFactory(orderService).getProxyInstance();
        String saveFileName = "CglibOrderServiceDynamicProxy.class";
        ProxyUtils.saveProxyClass(saveFileName, orderService.getClass().getSimpleName(), new Class[]{IOrderService.class});
        orderServiceProxy.saveOrder(" cruder 新买的花裤衩 ");
    }
}
```

cglib动态代理实现步骤和jdk动态代理及其相似，可以分为以下几个步骤：

1. 创建一个实现MethodInterceptor接口的类
2. 在类中定义一个被代理对象的成员属性，为了扩展方便可以直接使用Object类，也可以根据需求定义相应的接口
3. 在invoke方法中实现对委托对象的调用，根据需求对方法进行增强
4. 使用Enhancer创建生成代理对象，并提供要给获取代理对象的方法

cglib动态代理生成的代理类和jdk动态代理代码格式上几乎没有什么区别，唯一的区别在于cglib生成的代理类继承了仅仅Proxy类，而jdk动态代理生成的代理类继承了Proxy类的同时也实现了一个接口。代码如下：

```java
// 生成一个Proxy的子类
public final class OrderService extends Proxy {
    private static Method m1;
    private static Method m2;
    private static Method m0;

    public OrderService(InvocationHandler var1) throws  {
        super(var1);
    }

    public final boolean equals(Object var1) throws  {
        try {
            return (Boolean)super.h.invoke(this, m1, new Object[]{var1});
        } catch (RuntimeException | Error var3) {
            throw var3;
        } catch (Throwable var4) {
            throw new UndeclaredThrowableException(var4);
        }
    }

    public final String toString() throws  {
        try {
            return (String)super.h.invoke(this, m2, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    public final int hashCode() throws  {
        try {
            return (Integer)super.h.invoke(this, m0, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    static {
        try {
            m1 = Class.forName("java.lang.Object").getMethod("equals", Class.forName("java.lang.Object"));
            m2 = Class.forName("java.lang.Object").getMethod("toString");
            m0 = Class.forName("java.lang.Object").getMethod("hashCode");
        } catch (NoSuchMethodException var2) {
            throw new NoSuchMethodError(var2.getMessage());
        } catch (ClassNotFoundException var3) {
            throw new NoClassDefFoundError(var3.getMessage());
        }
    }
}
```

### JDK vs CGLIB

JDK Proxy 的优势：

- 最小化依赖关系，减少依赖意味着简化开发和维护，JDK 本身的支持，可能比 cglib 更加可靠。
- 平滑进行 JDK 版本升级，而字节码类库通常需要进行更新以保证在新版 Java 上能够使用。
- 代码实现简单。

cglib 优势：

- 有的时候调用目标可能不便实现额外接口，从某种角度看，限定调用者实现接口是有些侵入性的实践，类似 cglib 动态代理就没有这种限制。
- 只操作我们关心的类，而不必为其他相关类增加工作量。

代理模式: 为其他对象提供一种代理以控制（隔离，使用接口）对这个对象的访问。

jdk动态代理生成的代理类继承了Proxy类并实现了被代理的接口；而cglib生成的代理类则仅继承了Proxy类。

jdk动态代理最大缺点：只能代理接口，既委托类必须实现相应的接口

cglib缺点：由于是通过“子类化”的方式， 所以不能代理final的委托类或者普通委托类的final修饰的方法。

## 其他

动态代理导致spring事务失效的场景： https://www.jianshu.com/p/3dd79531fe41