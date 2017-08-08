---
title: 81. Search in Rotated Sorted Array II
date: 2017-08-07 23:05:53
tags: LeetCode
categories: LeetCode
---

# 81. Search in Rotated Sorted Array II

Follow up for "Search in Rotated Sorted Array":
　　What if duplicates are allowed?
　　Would this affect the run-time complexity? How and why?

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.
(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).
Write a function to determine if a given target is in the array.
The array may contain duplicates.

<!--more-->

## 题意：

　　[33. Search in Rotated Sorted Array](http://blog.taoaili999.cn/2017/07/18/33-Search-in-Rotated-Sorted-Array/)的变形，就是对一个按升序排序的数组通过预先未知的某个旋转轴进行旋转，给定一个搜索的目标值。如果在数组中找到，返回true，否则返回false，但是数组中出现重复的元素。

## 思路：

　　解题方法类似[33. Search in Rotated Sorted Array](http://blog.taoaili999.cn/2017/07/18/33-Search-in-Rotated-Sorted-Array/)，但是有重复元素的存在，会造成左侧不是递增的状态，即num[l]<=num[mid]时，此时nums[mid]==nums[l],但是中间的值比二者都小，所以不递增，在进行二分查找确定递增区间，缩小查找范围的时候，一定先判断中间元素是否和两侧的元素相等，如果相等且不等于目标元素，则要移动相应的指针，再进行判断进行区间的划分。

　　出现重复的时候，因为重复的次数我们无从得知，原数组旋转折叠的位置我们也无从得知，所以最后折叠后的状态我们也就无从得知，所以我们无法去按着[33. Search in Rotated Sorted Array](http://blog.taoaili999.cn/2017/07/18/33-Search-in-Rotated-Sorted-Array/)的判断标准去判断哪部分有序，设想一种情形1，1, 1，2，1,1,1，恰好左中右都是1,这时我们 ++左下标，--右下标，为什么这样我们不会将这个值跳过呢？ 就是说，凭什么我们说除却这两个位置，在两者之间就一定还存在这个值？当然能，因为我们进入的条件是左中右相等，所以我们可以把首尾的值都略过，进行下一次循环。

```c++
class Solution {
public:
	bool search(vector<int>& nums, int target) {
		int len = nums.size();
		if (len == 0)
			return false;
		int l = 0, r = len - 1;
		while (l <= r)//可能只存在一个元素
		{
			int mid = (l + r) / 2;
			if (nums[mid] == target)
			{
				return true;
			}
			// the only difference from the first one, trickly case, just updat left and right
			if ((nums[l] == nums[mid]) && (nums[r] == nums[mid])) { ++l; --r; }//如果左中右都相等，则左右指针向中间靠拢
			else if (nums[l] <= nums[mid])//此处认为，只要不是左中右都相等，只和左侧相等，是可以划分区间
			{
				if (target >= nums[l] && target < nums[mid])
				{
					r = mid - 1;
				}
				else
					l = mid + 1;
			}
			else
			{
				if (target > nums[mid] && target <= nums[r])
				{
					l = mid + 1;
				}
				else
					r = mid - 1;
			}
		}
		return false;
	}
};
```

　　**另外一种去掉相等重复元素思路的代码。**

```c++
class Solution {
public:
	bool search(vector<int>& nums, int target) {
		int len = nums.size();
		if (len == 0)
			return false;
		int l = 0, r = len - 1;
		while (l<=r)
		{
			int mid = (l + r) / 2;
			if (nums[mid]==target)
			{
				return true;
			}
			if (nums[l] < nums[mid])//没有判断nums[l] == nums[mid]，而是作为一个分支
			{
				if (target >= nums[l] && target < nums[mid])
				{
					r = mid - 1;
				}
				else
					l = mid + 1;
			}
			else if (nums[l] > nums[mid])
			{
				if (target > nums[mid] && target <= nums[r])
				{
					l = mid + 1;
				}
				else
					r = mid - 1;
			}
			else
				l++;//这里只根据nums[l] == nums[mid]来让l++，所以可能多几次循环遍历，但思路明确
		}
		return false;
	}
};
```

