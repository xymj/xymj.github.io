---
title: 50. Pow(x, n)
date: 2019-12-03 22:25:15
tags: LeetCode
categories: LeetCode
---

# 50. Pow(x, n)

Implement pow(x, n), which calculates x raised to the power(x,n).

Example 1:

Input: 2.00000, 10
Output: 1024.00000
<!--more-->
Example 2:

Input: 2.10000, 3
Output: 9.26100

Example 3:

Input: 2.00000, -2
Output: 0.25000
Explanation: 2-2 = 1/22 = 1/4 = 0.25

Note:

    -100.0 < x < 100.0
    n is a 32-bit signed integer, within the range [−231, 231 − 1]

## 题意：

　	实现 pow(x, n) ，即计算 x 的 n 次幂函数。

## 思路：

　　使用折半计算，每次把n缩小一半，这样n最终会缩小到0，任何数的0次方都为1，这时候我们再往回乘，如果此时n是偶数，直接把上次递归得到的值算个平方返回即可；如果是奇数，需要判断n是否为负数，n是负数的情况，需要把上次递归得到的值平方后乘以1/x，n是正数的情况，需要把上次递归得到的值平方后乘以x。

'''
思想： 使用折半计算。
    例如x=2,n=10
		res = 1, i = n

    i = 10     x = 2
    i = 5      res *= x, x = 4
    i = 2      x = 16
    i = 1      res = res *x
    即：
    i = 10     2,2,2,2,2,2,2,2,2,2
    i = 5      4,4,4,4,4   res=4
    i = 2      16,16
    i = 1      256,   res = 4 * 256
    i = 0      256*256    i不满足条件，循环停止

'''

```Java
class Solution {
    public double myPow(double x, int n) {
        if (n == 0) {
            return 1;
        }

        double minPowValue = myPow(x, n / 2);
        if (n % 2 != 0) {
            x = n < 0 ? 1 / x * minPowValue * minPowValue : x * minPowValue * minPowValue;
        } else {
            x = minPowValue * minPowValue;
        }
        return x;
    }
}
```
