---
title: 119. Pascal's Triangle II
date: 2017-08-19 12:14:27
tags: LeetCode
categories: LeetCode
---

# 119. Pascal's Triangle II

Given an index k, return the kth row of the Pascal's triangle.

For example, given k = 3,
Return [1,3,3,1].

Note:
Could you optimize your algorithm to use only O(k) extra space? 

<!--more-->

## 题意：

　　给定一个索引K，返回的第k行杨辉三角形的值。

## 思路：

　　思路同[118. Pascal's Triangle](http://blog.taoaili999.cn/2017/08/18/118-Pascal-s-Triangle/)，利用上一行三角形的值构造下一行，但是此题是只需要第k行的值，所以不用建造二维数组浪费空间，只需要用以为数组，通过记录上一行的值，然后从后往前遍历记录值，构造下一行值，其中有一点**注意**在遍历上行值求本行值的时候，本行因为比上一行多一个元素，所以索引也应该从比上一行元素值多一个的索引处开始，然后往前遍历求本行值。
　　利用每一行多出来的最后的位置开始来累加值，从后往前遍历结果数组的方法和思想很值得借鉴,而且代码看起来很简洁。

```c++
class Solution {
public:
	vector<int> getRow(int rowIndex) {
		vector<int> res(rowIndex+1,0);
		res[0] = 1;
		for (int i =1;i<=rowIndex;i++)
		{
			for (int j =i;j>=1;j--)
			{
				res[j] += res[j - 1];
			}
		}
		return res;
	}
};
```

　　 