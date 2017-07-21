---
title: 46. Permutations
date: 2017-07-20 22:07:01
tags: LeetCode
categories: LeetCode
---

# 46. Permutations

Given a collection of distinct numbers, return all possible permutations.

For example,
[1,2,3] have the following permutations:
[
　[1,2,3],
　[1,3,2],
　[2,1,3],
　[2,3,1],
　[3,1,2],
　[3,2,1]
]

<!--more-->

## 题意：

　　给定一组不同的数字，求出所有可能的排列，也就是求出一个数组的序列的全排列。

## 思路：

### 　方法一：

　　这个题的主要解决办法就是递归回溯法，每递归一层，取数列中的一个元素，当满足原素组大小时，得到一个结果组合，放入结果集中，然后回溯上一层，继续下一个组合。**注意点**就是每次递归下一层的时候，一定要把上面已经遍历过的元素在flag标记数组中进行标记，防止下一层再选中此元素，出现重复元素，返回上一层切记清除标记数组中的标记值。

```c++
class Solution {
public:
	vector<vector<int>> res;//结果集
	vector<vector<int>> permute(vector<int>& nums) {
		int len = nums.size();
		vector<int> flag(len, 0);//标记数组
		vector<int> tempRes;
		backtrackingPermute(nums, tempRes, flag, 0,len);
		return res;
	}
	void backtrackingPermute(vector<int> &nums, vector<int> tempRes,vector<int> flag, int index,int sizes) {
		if (index==sizes)
		{
			res.push_back(tempRes);
			return;
		}
		for (int i = 0;i<sizes;i++)
		{
			if (!flag[i])//没有被标记的元素才能放入组合数组中，进行递归下一层
			{
				flag[i] = 1;//添加标记
				tempRes.push_back(nums[i]);
				backtrackingPermute(nums, tempRes, flag, index + 1, sizes);
				tempRes.pop_back();
				flag[i] = 0;//清除标记
			}
		}
	}
};
```

### 　方法二：

　　递归回溯法，与方法一不同的地方是不使用标记数组，递归函数内部的for循环的起始索引是上层的index值，在循环内部进行两次swap交换操作，在递归前的交换可以得到一种递归结果，地递归后交换，可以使序列返回到 i 所在层的递归前状态，这样交转的方式，也能穷举出所有的元素排列组合。

```c++
class Solution {
public:
	vector<vector<int>> res;
	vector<vector<int>> permute(vector<int>& nums) {
		int len = nums.size();
		backtrackingPremute(nums, 0, len);
		return res;
	}
	void backtrackingPremute(vector<int> &nums, int index, int sizes) {
		if (index == sizes)
		{
			res.push_back(nums);
		}
		for (int i = index;i<sizes;i++)//索引从index开始遍历
		{
			swap(nums[i], nums[index]);//递归前交换
			backtrackingPremute(nums, index + 1, sizes);
			swap(nums[i], nums[index]);//递归后交换
		}
	}
};
```

### 　方法三：

　　直接利用C++的STL函数库next_permutation()函数，函数详情[next_permutation()](http://www.cplusplus.com/reference/algorithm/next_permutation/) 。

```c++
class Solution3 {
public:
	vector<vector<int>> permute(vector<int>& nums) {
		int len = nums.size();
		vector<vector<int>> res;
		sort(nums.begin(), nums.end(),greater<int>());//先对原数组排序
		res.push_back(nums);
		while (next_permutation(nums.begin(),nums.end(),greater<int>()))
		{
			res.push_back(nums);
		}
		return res;
	}
};
```