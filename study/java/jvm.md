## Java 运行时的内存划分

![Alt jvm_1](../../_media/jvm/jvm_1.jpg)
### 程序计数器

记录当前线程所执行的字节码行号，用于获取下一条执行的字节码。

当多线程运行时，每个线程切换后需要知道上一次所运行的状态、位置。由此也可以看出程序计数器是每个线程**私有**的。


### 虚拟机栈
虚拟机栈由一个一个的栈帧组成，栈帧是在每一个方法调用时产生的。

每一个栈帧由`局部变量区`、`操作数栈`等组成。每创建一个栈帧压栈，当一个方法执行完毕之后则出栈。

> - 如果出现方法递归调用出现死循环的话就会造成栈帧过多，最终会抛出 `StackOverflowError`。
> - 若线程执行过程中栈帧大小超出虚拟机栈限制，则会抛出 `StackOverflowError`。
> - 若虚拟机栈允许动态扩展，但在尝试扩展时内存不足，或者在为一个新线程初始化新的虚拟机栈时申请不到足够的内存，则会抛出
 `OutOfMemoryError`。

**这块内存区域也是线程私有的。**

### Java 堆
`Java` 堆是整个虚拟机所管理的最大内存区域，所有的对象创建都是在这个区域进行内存分配。

可利用参数 `-Xms -Xmx` 进行堆内存控制。

这块区域也是垃圾回收器重点管理的区域，由于大多数垃圾回收器都采用`分代回收算法`，所有堆内存也分为 `新生代`、`老年代`，可以方便垃圾的准确回收。

**这块内存属于线程共享区域。**

### 方法区(JDK1.7)

方法区主要用于存放已经被虚拟机加载的类信息，如`常量，静态变量`。
这块区域也被称为`永久代`。

可利用参数 `-XX:PermSize -XX:MaxPermSize` 控制初始化方法区和最大方法区大小。



### 元数据区(JDK1.8)

在 `JDK1.8` 中已经移除了方法区（永久代），并使用了一个元数据区域进行代替（`Metaspace`）。

默认情况下元数据区域会根据使用情况动态调整，避免了在 1.7 中由于加载类过多从而出现 `java.lang.OutOfMemoryError: PermGen`。

但也不能无限扩展，因此可以使用 `-XX:MaxMetaspaceSize`来控制最大内存。





### 运行时常量池

运行时常量池是方法区的一部分，其中存放了一些符号引用。当 `new` 一个对象时，会检查这个区域是否有这个符号的引用。



### 直接内存



直接内存又称为 `Direct Memory（堆外内存）`，它并不是由 `JVM` 虚拟机所管理的一块内存区域。

有使用过 `Netty` 的朋友应该对这块并内存不陌生，在 `Netty` 中所有的 IO（nio） 操作都会通过 `Native` 函数直接分配堆外内存。

它是通过在堆内存中的 `DirectByteBuffer` 对象操作的堆外内存，避免了堆内存和堆外内存来回复制交换复制，这样的高效操作也称为`零拷贝`。

既然是内存，那也得是可以被回收的。但由于堆外内存不直接受 `JVM` 管理，所以常规 `GC` 操作并不能回收堆外内存。它是借助于老年代产生的 `fullGC` 顺便进行回收。同时也可以显式调用 `System.gc()` 方法进行回收（前提是没有使用 `-XX:+DisableExplicitGC` 参数来禁止该方法）。

**值得注意的是**：由于堆外内存也是内存，是由操作系统管理。如果应用有使用堆外内存则需要平衡虚拟机的堆内存和堆外内存的使用占比。避免出现堆外内存溢出。


### 常用参数

![Alt jvm_2](../../_media/jvm/jvm_2.jpg)

通过上图可以直观的查看各个区域的参数设置。

常见的如下：

- `-Xms64m` 最小堆内存 `64m`.
- `-Xmx128m` 最大堆内存 `128m`.
- `-XX:NewSize=30m` 新生代初始化大小为`30m`.
- `-XX:MaxNewSize=40m` 新生代最大大小为`40m`.
- `-Xss=256k` 线程栈大小。
- `-XX:+PrintHeapAtGC` 当发生 GC 时打印内存布局。 
- `-XX:+HeapDumpOnOutOfMemoryError` 发送内存溢出时 dump 内存。


新生代和老年代的默认比例为 `1:2`，也就是说新生代占用 `1/3`的堆内存，而老年代占用 `2/3` 的堆内存。

可以通过参数 `-XX:NewRatio=2` 来设置老年代/新生代的比例。

## 类加载机制

### 双亲委派模型

模型如下图：

![Alt jvm_3](../../_media/jvm/jvm_3.jpg)

双亲委派模型中除了启动类加载器之外其余都需要有自己的父类加载器

当一个类收到了类加载请求时: 自己不会首先加载，而是委派给父加载器进行加载，每个层次的加载器都是这样。

所以最终每个加载请求都会经过启动类加载器。只有当父类加载返回不能加载时子加载器才会进行加载。

双亲委派的好处 : 由于每个类加载都会经过最顶层的启动类加载器，比如 `java.lang.Object`这样的类在各个类加载器下都是同一个类(只有当两个类是由同一个类加载器加载的才有意义，这两个类才相等。)

如果没有双亲委派模型，由各个类加载器自行加载的话。当用户自己编写了一个 `java.lang.Object`类，那样系统中就会出现多个 `Object`，这样 Java 程序中最基本的行为都无法保证，程序会变的非常混乱。


## OOM 分析

### Java 堆内存溢出

在 Java 堆中只要不断的创建对象，并且 `GC-Roots` 到对象之间存在引用链，这样 `JVM` 就不会回收对象。

只要将`-Xms(最小堆)`,`-Xmx(最大堆)` 设置为一样禁止自动扩展堆内存。


当使用一个 `while(true)` 循环来不断创建对象就会发生 `OutOfMemory`，还可以使用 `-XX:+HeapDumpOnOutOfMemoryError` 当发生 OOM 时会自动 dump 堆栈到文件中。

伪代码:

```java
    public static void main(String[] args) {
        List<String> list = new ArrayList<>(10) ;
        while (true){
            list.add("1") ;
        }
    }
```

当出现 OOM 时可以通过工具来分析 `GC-Roots` [引用链](https://github.com/crossoverJie/Java-Interview/blob/master/MD/GarbageCollection.md#%E5%8F%AF%E8%BE%BE%E6%80%A7%E5%88%86%E6%9E%90%E7%AE%97%E6%B3%95) ，查看对象和 `GC-Roots` 是如何进行关联的，是否存在对象的生命周期过长，或者是这些对象确实改存在的，那就要考虑将堆内存调大了。

```
Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
	at java.util.Arrays.copyOf(Arrays.java:3210)
	at java.util.Arrays.copyOf(Arrays.java:3181)
	at java.util.ArrayList.grow(ArrayList.java:261)
	at java.util.ArrayList.ensureExplicitCapacity(ArrayList.java:235)
	at java.util.ArrayList.ensureCapacityInternal(ArrayList.java:227)
	at java.util.ArrayList.add(ArrayList.java:458)
	at com.crossoverjie.oom.HeapOOM.main(HeapOOM.java:18)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at com.intellij.rt.execution.application.AppMain.main(AppMain.java:147)

Process finished with exit code 1

```
`java.lang.OutOfMemoryError: Java heap space`表示堆内存溢出。



更多内存溢出相关实战请看这里：[强如 Disruptor 也发生内存溢出？](https://crossoverjie.top/2018/08/29/java-senior/OOM-Disruptor/)




### MetaSpace (元数据) 内存溢出

> `JDK8` 中将永久代移除，使用 `MetaSpace` 来保存类加载之后的类信息，字符串常量池也被移动到 Java 堆。

`PermSize` 和 `MaxPermSize` 已经不能使用了，在 JDK8 中配置这两个参数将会发出警告。


JDK 8 中将类信息移到到了本地堆内存(Native Heap)中，将原有的永久代移动到了本地堆中成为 `MetaSpace` ,如果不指定该区域的大小，JVM 将会动态的调整。

可以使用 `-XX:MaxMetaspaceSize=10M` 来限制最大元数据。这样当不停的创建类时将会占满该区域并出现 `OOM`。

```java
    public static void main(String[] args) {
        while (true){
            Enhancer  enhancer = new Enhancer() ;
            enhancer.setSuperclass(HeapOOM.class);
            enhancer.setUseCache(false) ;
            enhancer.setCallback(new MethodInterceptor() {
                @Override
                public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
                    return methodProxy.invoke(o,objects) ;
                }
            });
            enhancer.create() ;

        }
    }
```
使用 `cglib` 不停的创建新类，最终会抛出:
```
Caused by: java.lang.reflect.InvocationTargetException
	at sun.reflect.GeneratedMethodAccessor1.invoke(Unknown Source)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at net.sf.cglib.core.ReflectUtils.defineClass(ReflectUtils.java:459)
	at net.sf.cglib.core.AbstractClassGenerator.generate(AbstractClassGenerator.java:336)
	... 11 more
Caused by: java.lang.OutOfMemoryError: Metaspace
	at java.lang.ClassLoader.defineClass1(Native Method)
	at java.lang.ClassLoader.defineClass(ClassLoader.java:763)
	... 16 more
```

注意：这里的 OOM 伴随的是 `java.lang.OutOfMemoryError: Metaspace` 也就是元数据溢出。

