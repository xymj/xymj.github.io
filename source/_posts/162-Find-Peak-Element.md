---
title: 162. Find Peak Element
date: 2017-08-26 11:50:08
tags: LeetCode
categories: LeetCode
---

A peak element is an element that is greater than its neighbors.
Given an input array where num[i] ≠ num[i+1], find a peak element and return its index.
The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

You may imagine that num[-1] = num[n] = -∞.
For example, in array [1, 2, 3, 1], 3 is a peak element and your function should return the index number 2.
Note:
Your solution should be in logarithmic complexity(对数复杂度).

<!--more-->

## 题意：

　　峰值元素是一种比它的左右邻居元素都大的元素。
　　给定的输入数组中的num[i] ≠ num [i+ 1 ]，找到一个峰值元素并返回它的值对应的索引。 
　　数组可能包含多个峰值，在这种情况下，返回任何一个峰值的索引都是好的。 
　　**注意：**
　　　　数组可能包含多个峰值，在这种情况下返回任何一个峰值的索引都是正确的。

## 思路：

　　找到数组中的一个峰值，并不是数组中的最大的那个峰值，所以可以利用二分查找的方法就行查找，时间复杂度会降低很多。

```c++
This problem is similar to Local Minimum. And according to the given condition, num[i] != num[i+1], there must exist a O(logN) solution. So we use binary search for this problem.

    If num[i-1] < num[i] > num[i+1], then num[i] is peak
    If num[i-1] < num[i] < num[i+1], then num[i+1...n-1] must contains a peak
    If num[i-1] > num[i] > num[i+1], then num[0...i-1] must contains a peak
    If num[i-1] > num[i] < num[i+1], then both sides have peak
    (n is num.length)
```

```c++
class Solution {
public:
	int findPeakElement(vector<int>& nums) {
		int len = nums.size();
		int l = 0, r = len - 1;
		while (l < r)
		{
			int mid = (l + r) / 2;
			if (l == mid)
			{
				if (nums[l] > nums[r])
					return l;
				else
					return r;
			}
			else if (nums[mid] > nums[mid + 1] && nums[mid] > nums[mid - 1])
			{
				return mid;
			}
			else if (nums[mid] < nums[mid + 1])
			{
				l = mid;
			}
			else if (nums[mid] < nums[mid - 1])
			{
				r = mid;
			}
		}
		return l;
	}
};
```

