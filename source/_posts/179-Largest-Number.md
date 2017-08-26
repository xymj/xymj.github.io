---
title: 179. Largest Number
date: 2017-08-26 16:41:40
tags: LeetCode
categories: LeetCode
---

Given a list of non negative integers, arrange them such that they form the largest number.
For example, given[3, 30, 34, 5, 9], the largest formed number is 9534330.
Note : The result may be very large, so you need to return a string instead of an integer.

<!--more-->

## 题意：

　　给定一组非负整数数组，排列组合这些整数，使其构成所有组合中最大的数。

## 思路：

　　分析如下：
　　　　eg1. [0, 0, 0 , 0] ->  0
　　　　eg2. [1211, 12] -> 121211而不是121112。
　　　　所以可以考虑排序，而排序的根据(也就是比较函数的比较原理)不是这些数的值的相对大小，而是它们构成新的数的大小比较。
　　　　所以，虽然1211 > 12， 但是
　　　　"1211" + "12" = "121112",
　　　　“12” + "1211" = "121211",
　　　　"121112" < "121211"
　　所以从构成新的数的角度来看, "1211" < "12" 。


　　对所有数进行排序，规则如下：
　　　　给定两个非负整数：a, b
　　　　将它们转换成字符串形式，然后进行连接。可得两种结果：
　　　　a在前，b在后，记为：strAB
　　　　b在前，a在后，记为：strBA
　　　　如果strAB > strBA，那么排序时a在b的前面。

　　其他的就是要注意全是0的情况和前导0的情况.

```c++
string largestNumber(vector<int>& nums) {
	if (nums.empty())
	{
		return NULL;
	}
	sort(nums.begin(), nums.end(), [](int a, int b) {return to_string(a) + to_string(b) > to_string(b) + to_string(a); });
	string res = "";
	for (auto rr : nums)
	{
		res += to_string(rr);
	}
   /* Input: [0, 0]
	Output : "00"
	Expected : "0"
	其他的就是要注意全是0的情况和前导0的情况
	*/
	//1
	/*if (res[0]=='0')
	{
		return "0";
	}
	else
		return res;*/
	//2
	auto pos = res.find_first_not_of('0');
	return pos == string::npos ? "0" : res;
	
}
```

