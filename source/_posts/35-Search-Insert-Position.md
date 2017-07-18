---
title: 35. Search Insert Position
date: 2017-07-18 22:49:16
tags: LeetCode
categories: LeetCode
---

# 35. Search Insert Position

Given a sorted array and a target value, return the index if the target is found.If not, return the index where it would be if it were inserted in order.
You may assume no duplicates in the array.

Here are few examples.
　[1, 3, 5, 6], 5 → 2
　[1, 3, 5, 6], 2 → 1
　[1, 3, 5, 6], 7 → 4
　[1, 3, 5, 6], 0 → 0
<!--more-->

## 题意：

　　给定一个排序数组和一个目标元素值，如果目标元素值在数组中，返回数组中的索引，如果不在数组中，返回其插入数组中的位置，并且插入后数组还是有序的。

## 思路：

### 　方法一：

　　直接利用迭代器进行遍历数组。

```c++
int searchInsert(vector<int>& nums, int target) {
	vector<int> ::iterator it = nums.begin();
	int count = 0;
	while (*it <= target) {
		if (*it == target) {
			return count;
		}
		++it;
		++count; 
		if (it == nums.end()) {
			nums.insert(it, target);
			return count;
		}
	}
	nums.insert(it, target);
	return count;
}
```

### 　方法一：

　　利用折半查找。

```c++
int searchInsert(vector<int>& nums, int target) {
	int n = nums.size();
	if (target > nums[n - 1]) {
		nums.push_back(target);
		return n;
	}
	int low, high, mid;
	low = 0;
	high = n - 1;
	while(low<=high){
		mid = (low + high) / 2;
		if (nums[mid] == target) {
			return mid;
		}
		if (nums[mid] > target)
			high = mid - 1;
		if (nums[mid] < target)
			low = mid + 1;
	}
	nums.insert(nums.begin() + low, target);
	return low;
}
```

　　