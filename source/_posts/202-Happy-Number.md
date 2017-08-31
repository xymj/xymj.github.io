---
title: 202. Happy Number
date: 2017-08-28 21:35:49
tags: LeetCode
categories: LeetCode
---

Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process : Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

Example : 19 is a happy number
​	1^2 + 9^2 = 82
​	8^2 + 2^2 = 68
​	6^2 + 8^2 = 100
​	1^2 + 0^2 + 0^2 = 1

<!--more-->

## 题意：

　　写一个算法来判断一个数字是否“happy”。
　　一个“happy”的数字是由下面的过程定义的一个数字：从任何正整数开始，用它的数字的平方代替数字，重复这个过程直到数字等于1，或者在一个不包括1的循环中循环。这个过程以1结尾的数字是“happy”数字。

## 思路：

　　递归循环求得数字的每一位，对每一位求平方再求和，求出来的和判断是否为1，如果为1循环结束，数字是happy数字，如果不为1，判断此和是否出现过，如果出现过说明求和出现循环，数字不是happy数字，如果没有重复出现过此和，则继续对此数字逐位求平方和。

```c++
bool isHappy(int n) {
	vector<int> digital;
	set<int> placeDig;
	int count = 0;
	while (1) {
		//cout <<"循环第"<< ++count << "次"<<endl;
		while (n) {
			int dig = n % 10;
			n /= 10;
			digital.push_back(dig);
		}
		int sum = 0;
		for (auto &num : digital) {
			sum += pow(num, 2);
		}
		//cout << "sum:::" << sum << endl;
		digital.clear();
		if (sum == 1)
			return true;
		auto res = placeDig.insert(sum);//判断此和是否循环出现过，set如果元素存在，insert()方法返回值的pair第二个元素为false
		if (res.second == false) {
			return false;
		}
		else
			n = sum;
	}
}
```

