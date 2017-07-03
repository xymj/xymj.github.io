---
title: 13. Roman to Integer
date: 2017-07-03 20:41:32
tags: LeetCode
categories: LeetCode
---

# 13. Roman to Integer

Given a roman numeral, convert it to an integer.
Input is guaranteed to be within the range from 1 to 3999.

## 题意：

　　给定一个罗马数字，将其转换为整数，输入的数字确保在1到3999之间。

<!--more-->

## 思路：

　　**罗马数字基本字符：**

1. I、V、X、L、C、D、M相应的阿拉伯数字表示为：1、5、10、50、100、500、1000

　　**罗马数字表示：**

1. 相同的数字连写，所表示的数等于这些数字相加得到的数，如：Ⅲ = 3；
2. 小的数字在大的数字的右边，所表示的数等于这些数字相加得到的数， 如：Ⅷ = 8；Ⅻ = 12；
3. 小的数字，（限于Ⅰ、X 和C）在大的数字的左边，所表示的数等于大数减小数得到的数，如：Ⅳ = 4；Ⅸ = 9；

　　**具体转换方法：**

1. 从左边第一个数字开始，依次加，因为有3的情况的存在， 加过以后判断，要是存在（3）的情况，就减去前一个（i - 1）＊2，因为本来应该减去前面这个小的数， 但是上次一循环还加了一次，于是这次就减去2次

2. 右加左減：

   (1). 在较大的罗马数字的右边记上小的罗马数字，表示大数字加小数字。在较大的罗马数字的左边记上小的罗马数字，表示大数字减去小数字。
   (2). 左减的数字有限制，仅限于I、X、C。比如45不可以写成VL，只能是XLV
   (3). 但是，左减不能跨去一个位数。比如，99不可以用IC（100 - 1）表示，而是用XCIX（[100 - 10] + [10 - 1]）表示。（等同于阿拉伯每位数字表示每位）
   (4). 左减数字必须为一位，比如8写成VIII，而非IIX。
   (5). 右加数字不可连续超过三位，比如14写成XIV，而非XIIII。

3. 所以用户输入一个数后，我们可以来遍历这个数，用sum来总计和，比较pre(s[i - 1]) 和 cur(s[i])，如果，cur 比 pre 小的话，直接相加，如果 cur 比 pre 大的话，则将总和sum减去pre这个地方数的两倍，同时加上 cur 就相当于后边的数比左边的数大，则用右边的数减左边的数。但因为之前已经加过一次了，所以减两次。

```c++
//方法一：从左往右遍历罗马数字
class Solution {
public:
	int romanToInt(string s) {
          //I、V、X、L、C、D、M相应的阿拉伯数字表示为：1、5、10、50、100、500、1000
          map<char, int>  m = { {'I',1},{ 'V',5 },{ 'X',10 },{ 'L',50 },{ 'C',100 },{ 'D',500 },{ 'M',1000 } };
          int n = s.size();
          int sum=0;
          for (int i = 0; i < n; i++)
              sum += m[s[i]];
          for (int i = 0; i < n - 1; i++) {
              if (m[s[i]] < m[s[i + 1]])
                  sum -= m[s[i]]*2;
          }
          return sum;
      }
};

//方法二：从右往左遍历罗马数字
class Solution {
public:
     int romanToInt(string s)
     {
          map<char, int>  m = { { 'I',1 },{ 'V',5 },{ 'X',10 },{ 'L',50 },{ 'C',100 },{ 'D',500 },{ 'M',1000 } };
          int res = 0;
          char max = 'I';
          for (int i = s.size() - 1; i >= 0; --i) {
              if (m[s[i]] >= m[max]) {
                  max = s[i];
                  res += m[s[i]];
              }
              else {
                  res -= m[s[i]];
              }
          }
          return res;
     }
};
```

