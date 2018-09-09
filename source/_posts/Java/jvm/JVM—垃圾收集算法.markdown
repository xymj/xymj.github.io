---
layout: "post"
title: "JVM—垃圾收集算法"
date: "2018-09-02 23:12"
tags:
  - Java
  - JVM
categories:
  - Java
---


### &nbsp; &nbsp;&nbsp;1、标记-清除算法(Mark-Sweep)
------
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;最基础的收集算法，之所以这么说，是因为下面的其它算法都是基于这种思路并对其不足进行改进而得到的。

&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;**原理：分为标记-清除两个阶段**<br/>
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.标记阶段：首先标记出所有需要回收的对象。<br/>
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.清除阶段：标记完成后，统一回收被标记的对象
<!--more-->
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;**缺点：**<br/>
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.效率问题：标记和清除过程效率都不高。<br/>
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.空间问题：标记清除之后会产生大量的不连续的内存碎片，空间碎片太多可能会导致以后在程序运行过程中需要分配较大的对象时，无法找到足够的连续的内存空间而不得不提前触发另一次垃圾收集动作。<br/>

![Mark-Sweep.png](/images/Mark-Sweep.png)



------
### &nbsp; &nbsp;&nbsp;2、复制算法(Copying)
------


&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;为了解决上面所提到的标记清楚算法的效率问题，而出现了复制算法。

&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;**原理：**<br/>
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.将现有的内存空间分为两快，每次只使用其中一块。<br/>
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.当其中一块内存用完的时候，就将还存活的对象复制到另外一块上去。
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.再把已使用过的内存空间一次清理掉。


&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;**优点：**<br/>
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.由于是每次都对整个半区进行内存回收，内存分配时不必考虑内存碎片问题。<br/>
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.只要移动堆顶指针，按顺序分配内存即可，实现简单，运行高效。<br/>


&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;**缺点：**<br/>
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.内存减少为原来的一半，太浪费了。<br/>
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.对象存活率较高的时候就要执行较多的复制操作，效率变低。<br/>
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.如果不使用50%的对分策略，老年代需要考虑的空间担保策略。<br/>

![Copying.png](/images/Copying.png)



&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;**内存划分优化：**<br/>
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;内存的划分并不需要根据1:1划分内存空间，而是将内存划分为一块较大的EdenSpace和两块较小的SurvivorSpace。

&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;JavaHeap内存回收模型（当前商业虚拟机大多使用此算法回收新生代）


![Copy-algorithm](/images/Copy-algorithm.png)

![eden-survivor](/images/eden-survivor.png)



------
### &nbsp; &nbsp;&nbsp;3、标记整理算法(Mark-Compact)
------


&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;由于复制算法的缺点，以及老年代的特点（**存活率高，没有额外内存对其进行空间担保**），老年代一般不使用复制算法。

&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;**原理：**<br/>
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1.标记阶段：首先标记出所有需要回收的对象，与“标记-清除”一样。<br/>
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.让存活的对象向内存的一端移动。而不像“标记-清除”算法那样直接对可回收对象进行清理。
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3.再直接清理掉端边界以外的内存。



![Mark-Compact](/images/Mark-Compact.png)




&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;由于老年代存活率高，没有额外内存对老年代进行空间担保，那么老年代只能采用标记-清理算法或者标记整理算法。　



------

### &nbsp; &nbsp;&nbsp;4、分代收集算法(Generational Collecting)
------
&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当前的商业虚拟机的垃圾收集都采用分代收集算法，把Java堆分为新生代和老年代。根据各个年代的特点采用最适当的收集算法。

&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在新生代中，每次垃圾收集时都发现有大批对象死去，只有少量存活，选用：复制算法

&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在老年代中因为对象存活率高、没有额外空间对它进行分配担保，就必须使用“标记-清除”或者“标记-整理”算法来进行回收。
