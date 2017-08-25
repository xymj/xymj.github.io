---
title: 153. Find Minimum in Rotated Sorted Array
date: 2017-08-24 23:20:05
tags: LeetCode
categories: LeetCode
---

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).
Find the minimum element.
You may assume no duplicate exists in the array.

<!--more-->

## 题意：

　　假设一个已排序的数组是在某个点（枢纽元素）上进行旋转，而且事先不知道枢纽元为什么的旋转。找出旋转后的数组的最小元素。**注意：没有重复元素存在。**

## 思路：

　　变形的二分查找，这和之前的[33. Search in Rotated Sorted Array](http://blog.taoaili999.cn/2017/07/18/33-Search-in-Rotated-Sorted-Array/)题型相同，只不过是查找的是最小元素，并不是查找给定的目标值，思路相同，设置首指针、尾指针，根据中间节点进行划分，逐渐缩小查找区域，最后找到最小值。

```c++
In this problem, we have only three cases.
Case 1. The leftmost value is less than the rightmost value in the list: This means that the list is not rotated(旋转).
e.g> [1 2 3 4 5 6 7 ]

Case 2. The value in the middle of the list is greater than the leftmost and rightmost values in the list.
e.g> [ 4 5 6 7 0 1 2 3 ]

Case 3. The value in the middle of the list is less than the leftmost and rightmost values in the list.
e.g> [ 5 6 7 0 1 2 3 4 ]

As you see in the examples above, if we have case 1, we just return the leftmost value in the list. If we have case 2, we just move to the right side of the list. If we have case 3 we need to move to the left side of the list.
```

```c++
class Solution {
public:
	int findMin(vector<int>& nums) {
		int len = nums.size();
		int left = 0, right = len - 1;
		while (left < right)
		{
			if (nums[left] < nums[right])
			{
				return nums[left];
			}
			int mid = (left + right) / 2;
			if (nums[right] < nums[mid])
			{
				left = mid + 1;
			}
			else
				right = mid;
		}
		return nums[left];
	}
};
```

