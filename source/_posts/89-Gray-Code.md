---
title: 89. Gray Code
date: 2017-08-09 22:38:35
tags: LeetCode
categories: LeetCode
---

# 89. Gray Code

The gray code is a binary numeral system where two successive values differ in only one bit.
Given a non-negative integer n representing the total number of bits in the code, print the sequence of gray code. A gray code sequence must begin with 0.

For example, given n = 2, return [0,1,3,2]. Its gray code sequence is:

00 - 0
01 - 1
11 - 3
10 - 2

Note:
For a given n, a gray code sequence is not uniquely defined.
For example, [0,2,3,1] is also a valid gray code sequence according to the above definition.
For now, the judge is able to judge based on one instance of gray code sequence. Sorry about that.

<!--more-->

## 题意：

　　格雷码是一个二进制数字系统，其中两个连续的值只在一个位上有所不同。给定一个非负整数n代表代码中的比特总数，打印格雷码序列。格雷码序列必须从0开始。

## 思路：

　　n+1位格雷码的集合 = n位格雷码集合(顺序)加前缀0 + n位格雷码集合(逆序)加前缀1。

```c++
//非递归的迭代实现
class Solution {
public:
	vector<int> grayCode(int n) {
		vector<int> res(1,0);
		for (int i = 1;i<=n;i++)
		{
			int sizes = res.size();
			for (int j = sizes-1; j >= 0; j--)
			{
				int val = res[j] | 1 << (i-1);//表示i位要使1往左移动i-1位，可以和逆序的i-1位的格雷码进行异或，其实就是实现计算n+1为格雷码时的n位格雷码集合(逆序)加前缀1
				res.push_back(val);
			}
		}
		return res;
	}	
};
```

```c++
//递归实现
class Solution {
public:
	vector<int> grayCode(int n) {
		vector<int> res;
		backtracking(res, n);
		return res;
	}
	void backtracking(vector<int> &res, int n) {
		if (n==0)
		{
			res.push_back(0);
			return;
		}
		backtracking(res, n - 1);
		int sizes = res.size();
		for (int i = sizes-1;i>=0;i--)
		{
			int val = res[i] | (1 << n - 1);
			res.push_back(val);
		}
	}
};
```

