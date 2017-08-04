---
title: 67. Add Binary
date: 2017-07-31 22:13:41
tags: LeetCode
categories: LeetCode
---

# 67. Add Binary

Given two binary strings, return their sum (also a binary string).

For example,
a = "11"
b = "1"
Return "100". 

<!--more-->

## 题意：

　　给定两个二进制字符串，返回它们的总和（和也是一个二进制字符串）。 

## 思路：

　　类似两个大整数相加，逐位相加，然后判断是否存在进位，用一个变量保存进位值，为下一位求和做准备。

```c++
//此法比较好理解，因为二进制进位情况也就四种，所以此法没有用求余数得到进位，而是直接条件判断决定进位和本文相加后的值
class Solution {
public:
	string addBinary(string a, string b) {
		string res = "";
		int aLen = a.size();
		int bLen = b.size();
		int len = max(aLen, bLen);
		a.insert(0, len - aLen, '0');
		b.insert(0, len - bLen, '0');
		int flag = 0;
		for (int i = len-1;i>=0;i--)
		{
			int num1 = a[i] - '0';
			int num2 = b[i] - '0';
			int sum = num1 + num2 + flag;
			if (sum == 3)
			{
				res += '1';
				flag = 1;
			}
			else if (sum==2)
			{
				res += '0';
				flag = 1;
			}
			else if (sum ==1)
			{
				res += '1';
				flag = 0;
			}
			else
			{
				res += '0';
				flag = 0;
			}
		}
		if (flag)
		{
			res += '1';
		}
		reverse(res.begin(), res.end());
		return res;
	}
};
```

```c++
//此法是通用做法，根据求余数确定本位求和后的值，根据相除确定进位值，当运算进制不同时，直接更改进制基数即可
class Solution {
public:
	string addBinary(string a, string b) {
		string result = "";
		int c = 0;
		int i = a.size() - 1;
		int j = b.size() - 1;
		while (i >= 0 || j >= 0 || c == 1)
		{
			c += i >= 0 ? a[i--] - '0' : 0;
			c += j >= 0 ? b[j--] - '0' : 0;
			result = char(c % 2 + '0') + result;
			c /= 2;

		}
		return result;
	}
};

```

