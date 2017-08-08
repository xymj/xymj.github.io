---
title: '80. Remove Duplicates from Sorted Array II '
date: 2017-08-07 22:26:14
tags: LeetCode
categories: LeetCode
---

# 80. Remove Duplicates from Sorted Array II

Follow up for "Remove Duplicates":
What if duplicates are allowed at most twice?

For example,
Given sorted array nums = [1,1,1,2,2,3],

Your function should return length = 5, with the first five elements of nums being 1, 1, 2, 2 and 3. It doesn't matter what you leave beyond the new length. 

<!--more-->

## 题意：

　　紧接着[26. Remove Duplicates from Sorted Array](http://blog.taoaili999.cn/2017/07/10/26-Remove-Duplicates-from-Sorted-Array/),只保留一个重复的元素的题，此题也是删除数组重复元素，但是重复元素最多允许出现两次。

## 思路：

　　[26. Remove Duplicates from Sorted Array](http://blog.taoaili999.cn/2017/07/10/26-Remove-Duplicates-from-Sorted-Array/)中只保留一个重复的元素，所以j从1开始，依次判读取的那个元素是否和j-1的元素是否相等，现在是两个重复的元素，应该j从2开始，比较j与j-1和j-2是否相等，[26. Remove Duplicates from Sorted Array](http://blog.taoaili999.cn/2017/07/10/26-Remove-Duplicates-from-Sorted-Array/)标志位从1开始，此题应该从2开始。

```c++
class Solution {
public:
	int removeDuplicates(vector<int>& nums) {
		int len = nums.size();
		if (len <=2)
		{
			return len;
		}
		int last = 2;//注意标志位的起始点，也就是后面不重复元素要放到前面覆盖重复元素的点
		for (int j =2; j < len; j++)
		{
			if (nums[j] != nums[last - 2]||nums[j]!=nums[last-1])
			{
				nums[last] = nums[j];
				last++;
			}
		}
		return  last;
	}
};
```

## 引申：

　　当可以重复3个、4个、、、、、n个呢，唯一的不同点就是标志位的选取有变换，当前元素和标志位之前元素比较各个数有变化，其他都是相同的，所以由公共的代码解法，适应任意多的重复元素个数。

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
       return allMethod(nums,2);
    }
    int allMethod(vector<int>& nums,int multiply){//第二个参数为排序数组中留下的重复元素的个数
        int len = nums.size();
        if(len<=multiply)
            return len;
        int tail = multiply;
        for(int i = multiply;i<len;i++)
        {
            if(nums[i]!=nums[tail-multiply])//此处优化之和前multiply个元素的第一个元素相比较，只要不相等，就可以覆盖tail所指向的元素，然后tail++，因为数组是递增有序的
                nums[tail++] = nums[i];
        }
        return tail;
    }
};
```

