---
title: 66. Plus One
date: 2017-07-27 22:47:49
tags: LeetCode
categories: LeetCode
---

# 66. Plus One

Given a non-negative number represented as an array of digits, plus one to the number.

The digits are stored such that the most significant digit is at the head of the list.
<!--more-->

## 题意：

　　给定一个非负数，表示为一个位数数组，在这个数字上加1。
　　加1后的数字被存储，使得最有意义的数字在列表的头部。

## 思路：

　　此题只要理解题意就很好做，题的意思就是要求是给定一个数组表示非负整数，其高位在数组的前面，对这个整数加1。简单的大数加法，遍历数组的每位，同时处理进位，如果最后还有进位，则在数组最前面在插入1即可。

```c++
class Solution2 {
public:
	vector<int> plusOne(vector<int>& digits) {
		int len = digits.size();
		int remainder = 1;
		for (int i = len-1;i>=0;i--)
		{
			int val = 0;
			val = digits[i] + remainder;
			if (val == 10)
			{
				remainder = 1;
				digits[i] = 0;
			}
			else
			{
				remainder = 0;
				digits[i] = val;
			}	
		}
		if (remainder)
		{
			digits.push_back(1);
			reverse(digits.begin(), digits.end());
		}
		return digits;
	}
};
```

```c++
class Solution {
public:
	vector<int> plusOne(vector<int>& digits) {
		int len = digits.size();
		for (int i = len - 1; i >= 0; i--)
		{
			if (digits[i]==9)
			{
				digits[i] = 0;
			}
			else
			{
				++digits[i];
				return digits;
			}
		}
		digits[0] = 1;
		digits.push_back(0);
		return digits;
	}
}
```

