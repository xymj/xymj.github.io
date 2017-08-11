---
title: 90. Subsets II
date: 2017-08-09 23:11:27
tags: LeetCode
categories: LeetCode
---

# 90. Subsets II

Given a collection of integers that might contain duplicates, nums, return all possible subsets.

Note: The solution set must not contain duplicate subsets.

For example,
If nums = [1,2,2], a solution is:
[
　　[2],
　　[1],
　　[1,2,2],
　　[2,2],
　　[1,2],
　　[]
]

<!--more-->

## 题意：

　　给定一个整数集合可能包含重复的数组元素，返回所有可能的子集。注意：解决方案集不能包含重复子集。

## 思路：

　　同[78.Subsets](http://blog.taoaili999.cn/2017/08/06/78-Subsets/)思路类似，也是通过回溯递归法实现，但是不同点是此处给的数组集合是包含重复元素的，但是结果姐不能出现重复的结果集，所以要想办法去重。
　　第一种去重的方法是首先对原数组进行排序，然后通过进入下层递归的地方进行判断，i==index的时候是新的一层递归的开始，所以有相同元素也没事，所以可以直接放入，当i!=index的时候是在此层前进的另外一种情况，所以要进一步判断i所对应的元素是否和i-1一样，如果一样则不能放到素组中，因为数组已经经过排序整理，再放进去肯定会出现重复的子集合。

```c++
class Solution {
public:
	vector<vector<int>> res;
	vector<vector<int>> subsetsWithDup(vector<int>& nums) {
		vector<int> tempRes;
		int sizes = nums.size();
		sort(nums.begin(), nums.end());//先排序
		backtracking(nums, tempRes, 0, sizes);
		return res;
	}
	void backtracking(vector<int>& nums, vector<int> tempRes, int index, int sizes) {
		//注意此处没有判断子集大小，因为所有子集都要查找出来，所以不需要加上额外的判断子集的大小
		res.push_back(tempRes);

		for (int i = index; i < sizes; i++)
		{
			if (i==index||nums[i]!=nums[i-1])//i==index的时候是新的一层递归的开始，所以可以直接放入，当i!=index的时候是在此层前进的另外一种情况，所以要进一步判断i所对应的元素是否和i-1一样，如果一样则不能放到素组中，因为数组已经经过排序整理
			{
				tempRes.push_back(nums[i]);
				backtracking(nums, tempRes, i + 1, sizes);
				tempRes.pop_back();
			}
		}
	}
};
```

　　第二种去重的方法是直接了利用C++的set集合的特性，set集合中不能出现重复元素，当插入已经存在的元素的时候会插入不成功，所以借助set集合去重，但是效率低一些，不像方法一进行剪枝，此方法会多运算重复元素循环递归构造子集的过程，提高时间复杂度。

```c++
class Solution3 {
public:
	set<vector<int>> res;//利用set集合去重，并没有剪枝，减少循环递归次数
	vector<vector<int>> subsetsWithDup(vector<int>& nums) {
		vector<int> tempRes;
		int sizes = nums.size();
		sort(nums.begin(), nums.end());
		for (int step = 0; step <= sizes; step++)
		{
			backtracking(nums, tempRes, step, sizes, 0);
		}
		vector<vector<int>> result;
		for (auto r : res)
		{
			result.push_back(r);
		}
		return result;
	}
	void backtracking(vector<int>& nums, vector<int> tempRes, int k, int sizes, int index) {
		if (k == 0)
		{
			res.insert(tempRes);
			return;
		}
		for (int i = index; i <= sizes - k; i++)
		{
			tempRes.push_back(nums[i]);
			backtracking(nums, tempRes, k - 1, sizes, i + 1);
			tempRes.pop_back();
		}
	}
};
```

