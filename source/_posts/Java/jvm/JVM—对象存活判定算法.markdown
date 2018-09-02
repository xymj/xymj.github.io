---
layout: "post"
title: "JVM—对象存活判定算法"
date: "2018-09-02 22:39"
tags:
  - Java
  - JVM
categories:
  - Java
---


### &nbsp; &nbsp;&nbsp;1、引用计数算法
------
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;引用计数算法就是对于创建的每一个对象都有一个与之关联的计数器，这个计数器记录着该对象被使用的次数，每当有一个地方引用它时，计数器值加1；当引用失效时，计数器减1；任何时刻计数器都为0的对象就是不可能再被使用的，垃圾收集器在进行垃圾回收时，对扫描到的每一个对象判断一下计数器是否等于0，若等于0，就会释放该对象占用的内存空间,同时将该对象引用的其他对象的计数器进行减一操作。


<!--more-->

&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;引用计数算法的垃圾收集一般有侵入式与非侵入式两种，侵入式的实现就是将引用计数器直接根植在对象内部，用C++的思想进行解释就是，在对象的构造或者拷贝构造中进行加一操作，在对象的析构中进行减一操作，非侵入式思想就是有一块单独的内存区域，用作引用计数器。

&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;算法的优点：引用计数算法的实现简单，判断效率也很高，使用引用计数器,内存回收可以穿插在程序的运行中，在程序运行中，当发现某一对象的引用计数器为0时，可以立即对该对象所占用的内存空间进行回收，这种方式可以避免FULL GC时带来的程序暂停。

&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;算法的缺点：采用引用计数器进行垃圾回收，最大的缺点就是不能解决**循环引用**的问题，例如一个象(A Object)持有另外一个对象(B Object)的引用，t同时B对象也持有A对象的引用，这种情况下，两个对象实例将一直存在于JVM的堆中，无法进行回收,代码示例如下(引用计数器无法对a与b对象进行回收):

```
class A {
    private B b;
    public B getB() {
        return b;
    }
    public void setB(B b) {
        this.b = b;
    }
}

class B {
    private A a;
    public A getA() {
        return a;
    }
    public void setA(A a) {
        this.a = a;
    }
}

public class Test {
    public static void main(String[] args) {
        A a = new A();
        B b = new B();
        a.setB(b);
        b.setA(a);
    }
}
```


&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;Java语言中没有选用引用计数算法来管理内存，其中最主要的一个原因也是它很难解决**对象之间相互循环引用的问题**。
```
/**
* 在testGC()方法中，对象objA和objB都有字段instance，
* 赋值令objA.instance=objB及objB.instance=objA，
* 除此之外这两个对象再无任何引用，
* 实际上这两个对象都已经不能再被访问，
* 但是它们因为相互引用着对象方，
* 导致它们的引用计数都不为0，
* 于是引用计数算法无法通知GC收集器回收它们。
*/  


/**
 * 执行后，objA和objB会不会被GC呢？
 */
public class ReferenceCountingGC {
    public Object instance = null;
    private static final int _1MB = 1024 * 1024;
    /**
     * 这个成员属性的唯一意义就是占点内存，以便能在GC日志中看清楚是否被回收过
     */
    private byte[] bigSize = new byte[2 * _1MB];
    public static void testGC() {
        ReferenceCountingGC objA = new ReferenceCountingGC();
        ReferenceCountingGC objB = new ReferenceCountingGC();
        objA.instance = objB;
        objB.instance = objA;
        objA = null;
        objB = null;

        //假设在这行发生了GC，objA和ojbB是否被回收
        System.gc();
    }
}
```
运行结果：

[Full GC (System)  [Tenured: 0K->210K(10240K), 0.0149142 secs] **4603K->210K**(19456K), [Perm : 2999K->2999(21248K)], 0.0150007 secs] [Times: user=0.01 sys=0.00, real=0.02 secs]

&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;从运行结果中可以看到GC日志中包含”4603K->210K”，这意味着虚拟机并没有因为这两个对象相互引用就不回收它们，这也证明虚拟并不是通过引用计数算法来判断对象是否存活的。

------
<br/>



### &nbsp; &nbsp;&nbsp;2、可达性分析算法
-----
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;在主流的商用程序语言中(Java和C#)，都是使用可达性分析(Reachability Analysis)算法判断对象是否存活的。这个算法的基本思路就是通过一系列名为”GC Roots”的对象作为起始点，从这些节点开始向下搜索，搜索所走过的路径称为引用链(Reference Chain)，当一个对象到GC Roots没有任何引用链相连时，则证明此对象是不可用的，下图对象object5, object6, object7虽然有互相判断，但它们到GC Roots是不可达的，所以它们将会判定为是可回收对象。


&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;![GCRootReachAnalysis.png](/images/GCRootReachAnalysis.png)

&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;在Java语言里，可作为GC Roots对象的包括如下几种：<br/>
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;1. 虚拟机栈(栈桢中的本地变量表)中的引用的对象<br/>
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;2. 方法区中的类静态属性引用的对象<br/>
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;3. 方法区中的常量引用的对象<br/>
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;4. 本地方法栈中JNI的引用的对象<br/>


####  &nbsp; &nbsp;&nbsp;对象生存还是死亡
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;即使在可达性分析算法中不可达的对象，也并非是“非死不可”的，这时候它们暂时处于“缓刑”阶段，要真正宣告一个对象死亡，至少要经历两次标记过程。<br/>
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;两次对对象进行标记的前提是对象在进行可达性分析后发现没有与GC Roots相连接的引用链。<br/>

&nbsp; &nbsp;&nbsp;&nbsp;**1).第一次标记并进行一次筛选。**<br/>
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;筛选的条件是此对象是否有必要执行finalize()方法。<br/>
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;当对象没有覆盖finalize方法，或者finzlize方法已经被虚拟机调用过，虚拟机将这两种情况都视为“没有必要执行”，对象被回收。<br/>

&nbsp; &nbsp;&nbsp;&nbsp;**2).第二次标记**<br/>
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;如果这个对象被判定为有必要执行finalize()方法，那么这个对象将会被放置在一个名为 F-Queue 的队列之中，并在稍后由一个虚拟机自动建立的、低优先级的Finalizer线程去执行。这里所谓的“执行”是指虚拟机会触发这个方法，但并不承诺会等待它运行结束。这样做的原因是，如果一个对象finalize()方法中执行缓慢，或者发生死循环（更极端的情况），将很可能会导致F-Queue队列中的其他对象永久处于等待状态，甚至导致整个内存回收系统崩溃。<br/>
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;finalize()方法是对象脱逃死亡命运的最后一次机会，稍后GC将对F-Queue中的对象进行第二次小规模标记，如果对象要在finalize()中成功拯救自己——只要重新与引用链上的任何的一个对象建立关联即可，譬如把自己(this 关键字)赋值给某个类变量或对象的成员变量，那在第二次标记时它将移除出“即将回收”的集合。如果对象这时候还没逃脱，那基本上它就真的被回收了。不过要尽量避免使用finalize这个方法。<br/>

流程图如下：

![finilized2.png](/images/finilized2.png)

![finilized1.png](/images/finilized1.png)

代码：

```
/**
 * 此代码演示了两点
 * 1、对象可以在被GC时自我拯救
 * 2、这种自救的机会只有一次，因为一个对象的finalize()方法最多只能被系统自动调用一次。
 */
public class FinalizeEscapeGC {
    public static FinalizeEscapeGC SAVE_HOOK = null;
    public void isAlive() {
        System.out.println("yes, I am still alive");
    }
    protected void finalize() throws Throwable {
        super.finalize();
        System.out.println("finalize method executed!");
        FinalizeEscapeGC.SAVE_HOOK = this;
    }
    public static void main(String[] args) throws InterruptedException {
        SAVE_HOOK = new FinalizeEscapeGC();
        //对象第一次成功拯救自己
        SAVE_HOOK = null;
        System.gc();
        //因为finalize方法优先级很低，所有暂停0.5秒以等待它
        Thread.sleep(500);
        if (SAVE_HOOK != null) {
            SAVE_HOOK.isAlive();
        } else {
            System.out.println("no ,I am dead!");
        }
        //-----------------------
        //以上代码与上面的完全相同,但这次自救却失败了！！！
        SAVE_HOOK = null;
        System.gc();
        //因为finalize方法优先级很低，所有暂停0.5秒以等待它
        Thread.sleep(500);
        if (SAVE_HOOK != null) {
            SAVE_HOOK.isAlive();
        } else {
            System.out.println("no ,I am dead!");
        }
    }
}
```
运行结果：<br/>
finalize method executed!<br/>
yew, I am still alive<br/>
no ,I am dead!<br/>

&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;从结果可以看出，SAVE_HOOK对象的finalize()方法确实被GC收集器触发过，并且在被收集前成功逃脱了。

&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;另外一个值得注意的地方是，代码中两段完全一样的代码，执行结果却是一次逃脱成功，一次失败，这是因为**任何一个对象的finalize()方法都只会被系统自动调用一次**，如果对象面临下一次回收，它的finalize()方法不会被再次执行，因此第二段代码的自救行动失败了。**不建议自己重写finalize()方法，尽量避免使用。**

------
<br/>

####  &nbsp; &nbsp;&nbsp;3、引用
-----

&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;无论是通过引用计数算法判断对象的引用数量，还是通过根搜索算法判断对象的引用链是否可达，判定对象是否存活都与“引用”有关。在JDK 1.2之前，Java中的引用的定义很传统：如果reference类型的数据中存储的数值代表的是另外一块内存的起始地址，就称这块内存代表着一个引用。这种定义很纯粹，但是太过狭隘，一个对象在这种定义下只有被引用或者没有被引用两种状态，对于如何描述一些“食之无味，弃之可惜”的对象就显得无能为力。我们希望能描述这样一类对象：当内存空间还足够时，则能保留在内存之中；如果内存在进行垃圾收集后还是非常紧张，则可以抛弃这些对象。很多系统的缓存功能都符合这样的应用场景。

&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;在JDK 1.2之后，Java对引用的概念进行了扩充，将引用分为强引用（Strong Reference）、软引用（Soft Reference）、弱引用（Weak Reference）、虚引用（Phantom Reference）四种，这四种引用强度依次逐渐减弱。

&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;**强引用**：就是指在程序代码之中普遍存在的，类似Object obj = new Object()这类的引用，只要强引用还存在，垃圾收集器永远不会回收掉被引用的对象。

&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;**软引用**：用来描述一些还有用，但并非必需的对象。对于软引用关联着的对象，在系统将要发生内存溢出异常之前，将会把这些对象列进回收范围之中并进行第二次回收。如果这次回收还是没有足够的内存，才会抛出内存溢出异常。在JDK 1.2之后，提供了SoftReference类来实现软引用。

&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;**弱引用**：也是用来描述非必需对象的，但是它的强度比软引用更弱一些，被弱引用关联的对象只能生存到下一次垃圾收集发生之前。当垃圾收集器工作时，无论当前内存是否足够，都会回收掉只被弱引用关联的对象。在JDK 1.2之后，提供了WeakReference类来实现弱引用。

　　
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;**虚引用**：也称为幽灵引用或者幻影引用，它是最弱的一种引用关系。一个对象是否有虚引用的存在，完全不会对其生存时间构成影响，也无法通过虚引用来取得一个对象实例。为一个对象设置虚引用关联的唯一目的就是希望能在这个对象被收集器回收时收到一个系统通知。在JDK 1.2之后，提供了PhantomReference类来实现虚引用。
