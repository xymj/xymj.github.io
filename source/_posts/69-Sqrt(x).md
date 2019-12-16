---
title: 69. Sqrt(x)
date: 2019-12-16 22:13:41
tags: LeetCode
categories: LeetCode
---

# 69. Sqrt(x)

Implement int sqrt(int x).

Compute and return the square root of x, where x is guaranteed to be a non-negative integer.

Since the return type is an integer, the decimal digits are truncated and only the integer part of the result is returned.
<!--more-->
Example 1:

Input: 4
Output: 2

Example 2:

Input: 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since the decimal part is truncated, 2 is returned.




## 题意：

实现 int sqrt(int x) 函数。

计算并返回 x 的平方根，其中 x 是非负整数。

由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。

## 思路：

　　二分查找，逐步找出平方接近x的值，并且用除的方式来比较查找的平方根的目标值，而不用乘法，防止整形越界。


###方法一：
		性能不是很好，并且取了long来针对越界问题，不是很好的解决方法。

```Java
class Solution {
    public int mySqrt(int x) {
      if (x == 0) {
            return 0;
        }

        if (x == 1) {
            return 1;
        }

        int mid = (int)Math.ceil(x / 2.0);
        for (long i = 1;i <= mid;i++) {//如果i不设置为long，那么i * i会按整形计算，超出整形32位的数字将会被抛弃，然后再转换为long，只有long * long 才不会出现越界问题
            long pow = i * i;
            long morePow = (i + 1) * (i + 1);
            if (pow <= x && morePow > x) {
                return (int)i;
            }
        }

        return 1;
    }
}
```

###方法二：
```Java
class Solution {
    public int mySqrt(int x) {
       if (x == 0 || x == 1) {
           return x;
       }

       int left = 1;
       int right = x;
       int res = 1;
       while (left <= right) {
           int mid = (left + right) / 2;
           int sqrtVal = x / mid;
           if (mid == sqrtVal) {
               return mid;
           } else if (mid < sqrtVal) {
               left = mid + 1;
               res = mid;//记录下mid平方比x小的值，因为平方根取整后结果的平方肯定比x小
           } else {
               right = mid - 1;
           }
       }

       return res;
    }
}
```
