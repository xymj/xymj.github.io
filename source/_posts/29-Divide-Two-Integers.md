---
title: 29. Divide Two Integers
date: 2019-11-15 00:03:58
tags: LeetCode
categories: LeetCode
---

# 29. Divide Two Integers

Given two integers dividend and divisor, divide two integers without using multiplication, division and mod operator.

Return the quotient after dividing dividend by divisor.

The integer division should truncate toward zero.

Example 1:

Input: dividend = 10, divisor = 3
Output: 3

Example 2:

Input: dividend = 7, divisor = -3
Output: -2

Note:

    Both dividend and divisor will be 32-bit signed integers.
    The divisor will never be 0.
    Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. For the purpose of this problem, assume that your function returns 231 − 1 when the division result overflows.



<!--more-->

## 题意：

		给定两个整数，被除数 dividend 和除数 divisor。将两数相除，要求不使用乘法、除法和 mod 运算符。

		返回被除数 dividend 除以除数 divisor 得到的商。

## 注意：

		1. 被除数和除数均为 32 位有符号整数。
		2. 除数不为 0。
		3. 假设我们的环境只能存储 32 位有符号整数，其数值范围是 [−231,  231 − 1]。本题中，如果除法结果溢出，则返回 231 − 1。


## 思路：

```Java
		/**
		 * 解题思路：这题是除法，所以先普及下除法术语
		 * 商，公式是：(被除数-余数)÷除数=商，记作：被除数÷除数=商...余数，是一种数学术语。
		 * 在一个除法算式里，被除数、余数、除数和商的关系为：(被除数-余数)÷除数=商，记作：被除数÷除数=商...余数，
		 * 进而推导得出：商×除数+余数=被除数。
		 *
		 * 要求商，我们首先想到的是减法，能被减多少次，那么商就为多少，但是明显减法的效率太低
		 *
		 * 那么我们可以用位移法，因为计算机在做位移时效率特别高，向左移1相当于乘以2，向右位移1相当于除以2
		 *
		 * 我们可以把一个dividend（被除数）先除以2^n，n最初为31，不断减小n去试探,当某个n满足dividend/2^n>=divisor时，
		 *
		 * 表示我们找到了一个足够大的数，这个数*divisor是不大于dividend的，所以我们就可以减去2^n个divisor，以此类推
		 *
		 * 我们可以以100/3为例
		 *
		 * 2^n是1，2，4，8...2^31这种数，当n为31时，这个数特别大，100/2^n是一个很小的数，肯定是小于3的，所以循环下来，
		 *
		 * 当n=5时，100/32=3, 刚好是大于等于3的，这时我们将100-32*3=4，也就是减去了32个3，接下来我们再处理4，同样手法可以再减去一个3
		 *
		 * 所以一共是减去了33个3，所以商就是33
		 *
		 * 这其中得处理一些特殊的数，比如divisor是不能为0的，Integer.MIN_VALUE和Integer.MAX_VALUE
		 *
		 */
 ```


#### 　Java Code：
```java
class Solution {
    public int divide(int dividend, int divisor) {
        if (divisor == 0) {
            return 0;
        }

        if (dividend == Integer.MIN_VALUE && divisor == -1) {
            return Integer.MAX_VALUE;
        }

        boolean negative = (dividend ^ divisor) < 0;//用异或来计算是否符号相异

        long up = Math.abs((long)dividend); //注意转换为long类型
        long down = Math.abs((long)divisor);

        int result = 0;

        for (int i = 31;i >= 0;i--) {
            long divValue = (up >> i);
            if (divValue >= down) { //找出足够大的数2^n*divisor
                up -= (down << i); //将结果加上2^n
                result += (1 << i); //将被除数减去2^n*divisor
            }
        }

        return negative ? -result : result;  //符号相异取反
    }
}
```
