---
title: 11. Container With Most Water
date: 2017-06-30 17:07:57
tags: LeetCode
categories: LeetCode
---

# 11. Container With Most Water


Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container and n is at least 2. 

## 题意：

　　数组索引代表x轴坐标，数组元素值代表y轴索引，经过此点坐标做垂直于x轴的线段，求这样的线段，以及x轴围起来的容器能存储的最多的水。

## 思路：

　　从左右向中间遍历，左右两个指针依次向中间移动，在移动的过程中不断计算所围成容器的最大面积，其中有一点优化的地方就是当左右指针移动的时候，如果移动的下一个高度小于等于当前高度，直接移动，不用再计算面积，因为这样面经肯定会变小。

```c++
class Solution {
public:
	int maxArea(vector<int>& height) {
		int area = 0;
		int left = 0, right = height.size() - 1;
		while (left < right)
		{
			int ht = min(height[left], height[right]);
			area = max(area, ht*(right - left));
			while(height[left] <= ht&&left<right)//左边下一个高度小于当前高度，直接移动左边指针
			{
				left++;
			}
			while (height[right] <= ht&&left < right)//右边下一个高度小于当前高度，直接移动右边指针
			{
				right--;
			}
		}
		return area;
	}
};
```

