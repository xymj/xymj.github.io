---
title: 40. Combination Sum II
date: 2017-07-19 23:00:13
tags: LeetCode
categories: LeetCode
---

# 40. Combination Sum II

 Given a collection of candidate numbers (C) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.
Each number in C may only be used once in the combination.

Note:
　　All numbers (including target) will be positive integers.
　　The solution set must not contain duplicate combinations.

For example, given candidate set [10, 1, 2, 7, 6, 1, 5] and target 8,

A solution set is:
[
　　[1, 7],
　　[1, 2, 5],
　　[2, 6],
　　[1, 1, 6]
]

<!---more-->

## 题意：

　　题意同[39. Combination Sum](http://blog.taoaili999.cn/2017/07/19/39-Combination-Sum/),不同点就是，给定的数组元素含有重复元素值,而且每个元素在组合数组中只能出现一次。

## 思路：

　　思路同[39. Combination Sum](http://blog.taoaili999.cn/2017/07/19/39-Combination-Sum/),不同点就是，因为数组元素含有重复元素值，所以再递归的过程中，要进行判断，跳过重复元素。

```c++
class Solution {
	
public:
	vector<vector<int>> res;
	vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
		vector<int> sumNum;
		sort(candidates.begin(), candidates.end());
		backtracking(candidates, target, 0, 0, sumNum);
		return res;
	}
	void backtracking(vector<int> candidates, int target, int index, int sum, vector<int> sumNum) {
		if (sum == target)
		{
			res.push_back(sumNum);
			return;
		}
		else
		{
			for (int i = index; i < candidates.size(); i++)
			{
				if (sum + candidates[i] > target)//已经排序，可以剪枝
				{
					break;
				}
				if (i==index||candidates[i]!=candidates[i-1])//此处剪枝去除重复的元素，index是控制同一层元素，当i!=index得时候，肯定是同一层往后找同层元素，此时进行判断是否相等可以把重复元素去掉，因为已经排过序了
				{
					sumNum.push_back(candidates[i]);
					backtracking(candidates, target, i + 1, sum + candidates[i], sumNum);//此处索引i + 1取下一个元素，防止数组中一个元素取多次
					sumNum.pop_back();
				}
				
			}
		}
	}
};
```

