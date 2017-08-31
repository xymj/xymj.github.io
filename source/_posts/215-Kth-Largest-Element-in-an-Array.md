---
title: 215. Kth Largest Element in an Array
date: 2017-08-30 22:40:46
tags: LeetCode
categories: LeetCode
---

Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

For example,
Given [3,2,1,5,6,4] and k = 2, return 5.

Note:
You may assume k is always valid, 1 ≤ k ≤ array's length.

<!--more-->

## 题意：

　　查找未排序数组中的第k个最大元素。 请注意，它是排序顺序中的第k个最大元素，而不是第k个不同的元素。

## 思路：

　　题意要理解：第k大的元素时从最大边开始数起，最大的为第一大，次大的为第二大。。。。
　　　　　　　　378题的第k小的元素时从最小边开始数起，最小的为第一小，次小的为第二小。。。

### 　方法一：

　　利用优先队列做，也就是建立最小堆（或者最大堆）来求第k大的元素。[priority_queue容器详解](http://www.cplusplus.com/reference/queue/priority_queue/?kw=priority_queue) 。

```c++
class Solution {
public:
	int findKthLargest(vector<int>& nums, int k) {
		int res = 0;
		//最小堆的实现
		//priority_queue<int, vector<int>, greater<int>> queMin;//functional
		//for (auto n : nums)
		//{
		//	queMin.push(n);
		//	if (queMin.size()>k)
		//	{
		//		queMin.pop();
		//	}
		//}
		//return queMin.top();

		//最大堆的实现
		priority_queue<int, vector<int>, less<int>> queMax;//== priority_queue<int> queMax   默认构建最大堆
		for (auto n : nums)
		{
			queMax.push(n);
		}
		while (k>1)
		{
			queMax.pop();
			k--;
		}
		return queMax.top();
	}
};
```

### 　方法二：

　　直接利用c++的STL库nth_element。[nth_element函数详解](http://www.cplusplus.com/reference/algorithm/nth_element/?kw=nth_element) 。

```c++
class Solution {
public:
	int findKthLargest(vector<int>& nums, int k) {
		nth_element(nums.begin(), nums.begin() + k, nums.end());
		return *(nums.begin() + k);
	}
};
```

### 　方法三：

　　利用QuickSelect的方法实现方法二的查找第k个元素的方法，快速查找的思想和快排相同，快排具体详解见[快排实现](http://blog.taoaili999.cn/2017/06/20/QuickSort-implement-with-C++/)。

```c++
class Solution {
public:
	int findKthLargest(vector<int>& nums, int k) {
		quickSelect(nums, 0, nums.size() - 1, nums.size()-k);//因为下面排序是从小到大，所以找nums.size()-k处元素
		return nums[nums.size()-k];
	}
	
	int getPartition(vector<int>& nums, int low, int height) {
		int keyVal = nums[height];
		int i = low - 1;
		while (low<height)
		{
			if (nums[low]<keyVal)
			{
				i = i + 1;
				swap(nums[i], nums[low]);
			}
			low++;
		}
		swap(nums[i + 1], nums[height]);
		return i + 1;
	}
	bool quickSelect(vector<int>& nums,int low,int height, int k)
	{
		if (k<low||k>height)
		{
			return false;
		}
		//三元区中项的思想优化快选中项
		int mid = (low + height) / 2;
		if (nums[low]>nums[height])
		{
			swap(nums[low], nums[height]);
		}
		if (nums[mid]>nums[height])
		{
			swap(nums[mid], nums[height]);
		}
		if (nums[low] > nums[mid])
		{
			swap(nums[mid], nums[low]);
		}
		swap(nums[mid], nums[height]);
		int par = getPartition(nums, low, height);
		if (k==par)
		{
			return true;
		}
		if (k < par)
		{
			quickSelect(nums, low, par-1, k);
		}
		else
			quickSelect(nums, par+1, height, k);
	}
};
```

