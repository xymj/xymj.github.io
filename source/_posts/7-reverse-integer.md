---
title: "7.Reverse Integer"
date: "2019-10-11 23:18"
tags: LeetCode
categories: LeetCode
---


# 7. Reverse Integer

Given a 32-bit signed integer, reverse digits of an integer.

**Example 1:**

Input: 123

Output: 321

**Example 2:**

Input: -123

Output: -321

**Example 3:**

Input: 120

Output: 21

Note:
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.


<!--more-->

## 题意：

　　给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

## 注意：

  假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。


```java
class Solution {
    public int reverse(int x) {
        if (x <= Integer.MIN_VALUE || x >= Integer.MAX_VALUE) {
            return 0;
        }

        //Integer.MAX_VALUE = 2147483647
        //Integer.MIN_VALUE = -2147483648
        int maxCeiling = Integer.MAX_VALUE / 10;
        int minFloor = Integer.MIN_VALUE / 10;

        int res = 0;
        while (x != 0) {
            if (res > maxCeiling) {
                return 0;
            }
            if (res < minFloor) {
                return 0;
            }

            int digit = x % 10;
            if (res == maxCeiling && digit > 7) {
                return 0;
            }
            if (res == minFloor && digit < -8) {
                return 0;
            }

            x /= 10;
            res = res * 10 + digit;
        }

        return res;
    }
}

```

```java
class Solution {
    public int reverse(int x) {
        if (x <= Integer.MIN_VALUE || x >= Integer.MAX_VALUE) {
            return 0;
        }

        //Integer.MAX_VALUE = 2147483647
        //Integer.MIN_VALUE = -2147483648
        int maxCeiling = Integer.MAX_VALUE / 10;
        int minFloor = Integer.MIN_VALUE / 10;

        long res = 0;
        while (x != 0) {
            int digit = x % 10;
            x /= 10;
            res = res * 10 + digit;

            if (res <= Integer.MIN_VALUE || res >= Integer.MAX_VALUE) {
               return 0;
            }
        }

        return (int)res;
    }
}

```
