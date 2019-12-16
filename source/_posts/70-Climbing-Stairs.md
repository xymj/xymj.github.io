---
title: 70. Climbing Stairs
date: 2019-12-16 23:29:41
tags: LeetCode
categories: LeetCode
---

# 70. Climbing Stairs

You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?
<!--more-->
Note: Given n will be a positive integer.

Example 1:

Input: 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps

Example 2:

Input: 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step




## 题意：

假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

注意：给定 n 是一个正整数。

## 思路：

　　简单的动态规划思想，每一阶楼梯踏上的次数等于n-1阶和n-2的次数之和。

```Java
class Solution {
    public int climbStairs(int n) {
       if (n == 1 || n == 2) {
           return n;
       }

       //return climbStairs(n - 1) + climbStairs(n - 2);递归超时
       //使用数组存储到达每阶楼梯的方法次数
       int[] dp = new int[n + 1];
       dp[1] = 1;
       dp[2] = 2;
       for (int i = 3;i <= n;i++) {
           dp[i] = dp[i - 1] + dp[i - 2];
       }

       return dp[n];
    }
}
```
