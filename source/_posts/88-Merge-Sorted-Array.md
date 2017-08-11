---
title: 88. Merge Sorted Array
date: 2017-08-09 22:21:09
tags: LeetCode
categories: LeetCode
---

# 88. Merge Sorted Array

Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.

Note:
You may assume that nums1 has enough space (size that is greater or equal to m + n) to hold additional elements from nums2. The number of elements initialized in nums1 and nums2 are m and n respectively.

<!--more-->

## 题意：

　　 给定两个有序整数数组nums1和nums2，合并成一nums2和nums1，合并后也为排序数组。 

　　 **注意：**  可以认为nums1有足够的空间（尺寸大于或等于m + n）能够放下nums2的附加元素。元素在nums1和nums2初始化个数分别是m和n。 

## 思路：

### 　方法一：

　　从后往前在num1中放入元素，不借助辅助数组，一个索引指针tar记录m+n的位置，即合并后元素的末尾位置，然后从前往后遍历nums1和nums2，把其中打的元素直接放到tar处，然后相应数组索引指针减一，直到nums1和nums2的索引指针都为0，**切记注意**当nums1的索引指针先为0，而num2的索引指针不为0，那么最后一定要继续遍历num2，将其合并到num1中。

```c++
class Solution {
public:
	void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
		if (m == 0)
		{
			nums1 = nums2;
		}
		if (n == 0)
		{
			return;
		}
		int tar = m + n - 1;
		int p1 = m - 1, p2 = n - 1;
		while (p2 >= 0 && p1 >= 0)
		{
			if (nums1[p1] < nums2[p2])
			{
				nums1[tar--] = nums2[p2--];
			}
			else
				nums1[tar--] = nums1[p1--];
		}
		while (p2 >= 0)
		{
			nums1[tar--] = nums2[p2--];
		}
	}
};
```

### 　方法二：

　　从前往后遍历两个数组，一边遍历一边合并到临时数组中，这个方法需要申请额外的内存空间，代码思路类似归排序的归并过程。

```c++
class Solution {
public:
	void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
		if (m == 0)
		{
			nums1 = nums2;
		}
		if (n == 0)
		{
			return;
		}
		vector<int> tempNums1;
		int p1 = 0, p2 = 0;
		while (p1 < m&&p2 < n)
		{
			while (p1 < m&&nums1[p1] <= nums2[p2])
			{
				tempNums1.push_back(nums1[p1]);
				p1++;
			}
			while (p2 < n&&nums2[p2] <= nums1[p1])
			{
				tempNums1.push_back(nums2[p2]);
				p2++;
			}
		}
		while (p1 < m)
		{
			tempNums1.push_back(nums1[p1]);
			p1++;
		}
		while (p2 < n)
		{
			tempNums1.push_back(nums2[p2]);
			p2++;
		}
		for (int i = 0; i < tempNums1.size(); i++)
		{
			nums1[i] = tempNums1[i];
		}
	}
};
```

