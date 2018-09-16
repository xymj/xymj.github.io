---
layout: "post"
title: "CPU占用分析"
date: "2018-09-16 21:17"
tags:
  - Java
  - CPU
categories:
  - Java
---



## 一、概述

&ensp;&ensp;&ensp;当程序中出现死循环，或者计算量很大的线程的时候，就会导致Java程序进程占用大量CPU资源，从而可能导致程序挂掉，此文从实例进行概述具体的查询占用CPU资源高的程序代码。
<!--more-->

## 二、排查步骤
1. 查看占用CPU高的进程
    - top命令
2. 查看进程中占用CPU最多的线程。
    - top -Hp ${pid} 命令（PID表示进程ID，排列靠前的几个基本都是此进程下最占用CPU资源的线程）
3. 将进程信息转出到文件。
    - jstack -l ${pid} > ${file.log}
4. 将线程ID转成16进制。
    - printf '%x\n' ${TID} 命令（TID表示线程ID）
5. 根据16进制线程ID在3中输出的file.log中找到对应线程的息更具体代码信息。


## 三、具体实例

### 1、实例代码
&ensp;&ensp;&ensp;很简单的一个死循环例子，来模仿占用CPU资源，然后进行问题排查。
``` Java
public class CPUTestDemo {
    public static void main(String[] args) {
        int i = 0;
        while (true) {
            i++;
        }
    }
}
```


### 2、问题排查
&ensp;&ensp;&ensp;1)、启动程序，top命令查看当前占用CPU资源最高的进程，如下图：
![top.png](/images/top.png)
&ensp;&ensp;&ensp;从图中可以看出进程5278所占用的CPU资源99.5%，并且从COMMAND能看出确实是启动的Java进程，说明Java程序中存在问题，导致狂占CPU资源。

&ensp;&ensp;&ensp;2)、对1)中得到的占用CPU资源高的进程执行top -Hp 5278，得到此Java进程内最占用CPU资源的线程，如下图：
![top-Hp.png](/images/top-Hp.png)
&ensp;&ensp;&ensp;从图中可以看出线程5279所占用的CPU资源99.6%，这就可以明确的定位到Java程序中占用CPU资源的线程号5279。

&ensp;&ensp;&ensp;3)、对2)中得到的占用CPU资源高的线程号执行printf '%x\\n' 5279，得到线程号的十六进制值149f，如下图：
![tid-16Hx.png](/images/tid-16Hx.png)

&ensp;&ensp;&ensp;4)、通过jstack -l 5278 > file.log 重定向输出Java进程的jstack日志。

&ensp;&ensp;&ensp;5)、通过对3)中得到的线程十六进制值，在4)的file.log中查找线程中具体的占用CPU资源的代码信息，如下图：
![paramerror.png](/images/paramerror.png)
&ensp;&ensp;&ensp;从图中可以看出线程0x149f主线程中具体的占用CPU资源的代码行，定位到文字开始的实例代码，可以看到正常程序中的while死循环造成的CPU资源占用过高，从而定位代码问题。
