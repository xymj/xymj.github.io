---
title: 34. Search for a Range
date: 2017-07-18 22:29:20
tags: LeetCode
categories: LeetCode
---

# 34. Search for a Range

Given an array of integers sorted in ascending order, find the starting and ending position of a given target value.
Your algorithm's runtime complexity must be in the order of O(log n).
If the target is not found in the array, return [-1, -1].

For example,
Given [5, 7, 7, 8, 8, 10] and target value 8,
return [3, 4]. 

<!--more-->

## 题意：

　　给定一个按升序排序的整数数组，查找给定目标值的开始位置和结束位置。算法的运行时复杂性必须是O（log n）。如果数组中找不到目标，则返回[ 1，- 1 ]。

## 思路：

### 　方法一：

　　直接利用二分查找，找到指定的元素索引，因为是有序的非递减数组，所以相等的元素一定相邻，直接以此索引为中心，左右扩展，找到元素的起始位置和结束位置。

```c++
class Solution {
public:
	vector<int> searchRange(vector<int>& nums, int target) {
		vector<int> res = { -1,-1 };
		int len = nums.size();
		if (len == 0) return res;
		int targetIndex = binarySearchIndex(nums, target);
		if (targetIndex==-1)
		{
			return res;
		}
		int targetLeft = targetIndex, targetRight = targetIndex;
		while (targetLeft>=0)
		{
			if (nums[targetLeft] == target)
			{
				res[0] = targetLeft;
				targetLeft--;
			}
			else
				break;
		}
		while (targetRight < len)
		{
			if (nums[targetRight] == target)
			{
				res[1] = targetRight;
				targetRight++;
			}
			else
				break;
		}
		return res;
	}
	int binarySearchIndex(vector<int>& nums, int target)
	{
		int l = 0, r = nums.size()-1;//二分查找这个坐标的选取很重要，一定要指向要查找的第一个元素和最后一个元素！！！！！
		while (l<=r)//当元素出现的位置只有l==r才能确定时，比如[1,2]找2，一定会出现l==r，所以由上面左右坐标确定后，一定注意此处要有相等！！！！！！
		{
			int mid = (l + r) / 2;
			if (nums[mid] == target) return mid;
			if (target < nums[mid])   r = mid - 1;
			if (target > nums[mid])   l = mid + 1;
		}
		return -1;
	}
};

```

### 　方法二：

　　直接利用C++的STL函数库equal_range()，函数详情[equal_range()](http://www.cplusplus.com/reference/algorithm/equal_range/?kw=equal_range)。

```c++
class Solution {
public:
	vector<int> searchRange(vector<int>& nums, int target) {
		vector<int> res = { -1,-1 };
		int len = nums.size();
		if (len == 0) return res;
		pair<vector<int>::iterator, vector<int>::iterator> bound;
		bound = equal_range(nums.begin(), nums.end(), target);
		if (bound.first==nums.end()||*bound.first!=target)
		{
			return res;
		}
		res[0] = bound.first - nums.begin();
		res[1] = bound.second - nums.begin()-1;
		return res;
	}

};
```

### 　方法三：

　　直接利用C++的STL函数库lower_bound() 和upper_bound()函数，函数详情[lower_bound](http://www.cplusplus.com/reference/algorithm/lower_bound/)和[upper_bound()](http://www.cplusplus.com/reference/algorithm/upper_bound/)。

```c++
class Solution {
public:
	vector<int> searchRange(vector<int>& nums, int target) {
		vector<int> res = { -1,-1 };
		int len = nums.size();
		if (len == 0) return res;
		int low,up;
		low = lower_bound(nums.begin(), nums.end(), target) - nums.begin();
		if (low==len||nums[low] != target)
			return res;
		up = upper_bound(nums.begin(), nums.end(), target) - nums.begin() - 1;
		res[0] = low;
		res[1] = up;
		return res;
	}
};

```



