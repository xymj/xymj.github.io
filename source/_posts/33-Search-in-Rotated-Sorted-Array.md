---
title: 33. Search in Rotated Sorted Array
date: 2017-07-18 21:52:37
tags: LeetCode
categories: LeetCode
---

# 33. Search in Rotated Sorted Array

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.
(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).
You are given a target value to search. If found in the array return its index, otherwise return -1.
You may assume no duplicate exists in the array.

<!--more-->

## 题意：

　　假设一个按升序排序的数组通过预先未知的某个旋转轴进行旋转。
　　（即，0、1、2、4、6、7、5可能成为4、5、6、7、0、1、2）。
　　给定一个搜索的目标值。如果在数组中找到，返回它的索引，否则返回- 1。
　　假设数组中没有重复的元素。

## 思路：

　　二分查找的思路，从中间截取后一定会有一部分是有序的。左侧若有序，则右侧可能有序；左侧若无序，则右侧一定有序。判断左侧是否有序的方法是比较if (nums[left] <= nums[mid])。若左侧有序，此时target>nums[mid]则递归查找右侧，若target<nums[mid], 则比较target 与nums[left]的值，从而决定在哪里找target。
　　这个思路可以画图理解更清晰，y轴代表数组的值，x轴代表值多对应的数组下标，翻转后有如下三种情况，如下图：

![ThreeCases](/images/33ThreeCases.png)

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int len = nums.size();
		if (len == 0)
			return -1;
		int l = 0, r = len - 1;
		while (l <= r)//l和r可能指向同一个元素
		{
			int mid = (l + r) / 2;
			if (nums[mid] == target)
			{
				return mid;
			}
			if (nums[l] <= nums[mid])//l和m可能指向同一个元素
			{
				if (nums[mid] > target&&nums[l] <= target)
				{
					r = mid - 1;
				}
				else
					l = mid + 1;
			}
			else
			{
				if (target>nums[mid]&&target <= nums[r])
				{
					l = mid + 1;
				}
				else
				{
					r = mid - 1;
				}
			}
		}
		return -1;
    }
};
```

---------------------------------------------------
#### 　Java Code：
```Java
class Solution {
    public int search(int[] nums, int target) {
        if (null == nums) {
            return -1;
        }

        int len = nums.length;
        if (len == 0) {
            return -1;
        }

        int left = 0;
        int right = len - 1;
        while (left <= right) {
            int mid = (left + right) / 2;
            if (target == nums[mid]) {
                return mid;
            }

            if (nums[left] <= nums[mid]) {
                if (target >= nums[left] && target < nums[mid]) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            } else {
                if (target > nums[mid] && target <= nums[right]) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            }
        }

        return -1;
    }
}
```
