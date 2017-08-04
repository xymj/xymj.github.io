---
title: 75. Sort Colors
date: 2017-08-04 21:30:11
tags: LeetCode
categories: LeetCode
---

# 75. Sort Colors

Given an array with n objects colored red, white or blue, sort them so that objects of the same color are adjacent, with the colors in the order red, white and blue.
Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

Note:
You are not suppose to use the library's sort function for this problem. 
<!--more-->

## 题意：

　　给定一个包含红，白，蓝且长度为n的数组，将数组元素进行分类使同颜色的元素相邻，并按照红、白、蓝的顺序进行排序。
　　用整数0，1和2分别代表红，白，蓝。

## 思路：

### 　方法一：

　　利用计数排序，分别用三个变量countRed、countWhilte、countBlue来记录三种颜色各有几个。

```c++
void sortColors(vector<int>& nums) {
	int countRed=0, countWhilte=0, countBlue=0;
	for (int i=0;i<nums.size();i++)
	{
		if (nums[i] == 0)
		{
			countRed++;
		}
		else if (nums[i] == 1)
		{
			countWhilte++;
		}
		else
			countBlue++;
	}
	for (int i = 0; i < nums.size(); i++)
	{
		if (i >= 0 && i < countRed)
		{
			nums[i] = 0;

		}
		else if (i >= countRed&&i < countRed + countWhilte)
		{
			nums[i] = 1;
		}
		else
			nums[i] = 2;
	}
}
```

### 　方法二：

　　自己实现快速排序。

```c++
//1、获取中点值，划分块
int getPartition(vector<int> &array, int low, int height) {
	int valKey = array[low];
	while (low<height)
	{
		//必须先从height处开始比较，因为valKey记录的是第一个低位的值，放高位比低位小时，直接覆盖低位。
		while (low<height&&array[height]>=valKey)
		{
			height--;
		}
		array[low] = array[height];//直接是覆盖，不是交换
		while (low<height&&array[low]<=valKey)
		{
			low++;
		}
		//swap(array[low], array[height])//直接是覆盖，不是交换;
		array[height] = array[low];
	}
	array[low] = valKey;
	return low;
}
void quickSort(vector<int> &array, int low, int height) {
	if (low<height)
	{
		int partition = getPartition(array, low, height);
		quickSort(array, low, partition-1);
		quickSort(array, partition + 1, height);
	}
}
void sortColors(vector<int> &nums) {
	if (nums.empty())
	{
		return;
	}
	quickSort(nums, 0, nums.size() - 1);
}
```

### 　方法三：

　　利用三个指针low、mid、heigh，通过移动三个指针和交换指针所指向元素的值，low指针左边都保证是0(红色)，low指针和mid指针之间保证是1(白色)，heigh指针右面都保证是2(蓝色)。

```c++
void sortColors(vector<int> &nums) {
	if (nums.empty())
	{
		return;
	}
	int low = 0;
	int mid = 0;
	int heigh = nums.size() - 1;
	while (mid<=heigh)
	{
		switch (nums[mid])
		{
		case 0:
			swap(nums[low], nums[mid]);
			mid++;
			low++;
			break;
		case 1:
			mid++;
			break;
		case 2:
			swap(nums[mid], nums[heigh]);
			heigh--;
			break;
		default:
			break;
		}
	}
}
```

