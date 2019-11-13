---
title: 26. Remove Duplicates from Sorted Array
date: 2017-07-10 22:48:58
tags: LeetCode
categories: LeetCode
---

# 26. Remove Duplicates from Sorted Array

Given a sorted array, remove the duplicates in place such that each element appear only once and return the new length.
Do not allocate extra space for another array, you must do this in place with constant memory.

For example,
Given input array nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively. It doesn't matter what you leave beyond the new length.

<!--more-->

## 题意：

　　给定一个排序数组，删除重复的元素，使每个元素只出现一次，并返回新的长度。不要申请另一个额外的数组空间，必须在常数空间内，即在数组本身上完成去重。
　　返回值只需要返回不重复元素的数组元素长度即可。

## 思路：

　　利用一个前指针 i 和尾指针 count 遍历数组，当 i 所指向的元素值与相邻前一个元素相同，则直接往前移动 i 指针，当不同的时候，先移动 count  指针（因为count指针指向的值的是上一个不同值，所以先++移动），然后把 i 指向的值赋值给 count 所指向的值，最后不重复元素数组的长度即是count+1。

```c++
class Solution {
public:
	int removeDuplicates(vector<int>& nums) {
		int len = nums.size();
		if (len == 0)
		{
			return 0;
		}
		int count = 0;
		for (int i =1;i<len;i++)
		{
			if (nums[i]!=nums[i-1])
			{
				nums[++count] = nums[i];
			}
		}
		return count + 1;
	}
};
```

---------------------------------------------------
#### 　Java Code：
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        if (null == nums) {
            return 0;
        }
        int len = nums.length;
        if (len <= 1) {
            return len;
        }
        int slow = 0;
        for (int i = 1;i < len;i++) {
            if (nums[i] != nums[i - 1]) {//有序数组，比较相邻元素即可知道与前面是否相等
                slow++;
                nums[slow] = nums[i];
            }
        }

        return slow + 1;
    }
}
```
