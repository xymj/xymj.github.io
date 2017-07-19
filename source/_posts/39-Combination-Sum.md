---
title: 39. Combination Sum
date: 2017-07-19 22:46:23
tags: LeetCode
categories: LeetCode
---

# 39. Combination Sum

Given a set of candidate numbers (C) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.
The same repeated number may be chosen from C unlimited number of times.

Note:
　　All numbers (including target) will be positive integers.
　　The solution set must not contain duplicate combinations.

For example, given candidate set [2, 3, 6, 7] and target 7,
A solution set is:
[
　[7],
　[2, 2, 3]
]

<!---more-->

## 题意：

　　给定一组候选数字（C），数组中没有重复元素，和一个目标数（T），在C中找出所有唯一的组合，其中候选数总和为T。
同样的重复数可以从C无限次数选择。

## 思路：

　　主要是通过递归回溯来进行元素组合的查找，每次递归下一层，判断所取元素的和与目标值是否相等，相等则是一个结果，因为一个元素可以重复取，所以每次递归下层，数组的索引不用前进1，而且在递归回溯开始前先对数组进行排序，便于后面条件判断的剪枝，减少递归层数，提高效率。

```c++
class Solution {
public:
	vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
		vector<vector<int>> res;
		vector<int> sumNum;//记录组合元素
		sort(candidates.begin(), candidates.end());//先排序，便于剪枝，减少递归层数
		backtracking(candidates, target, 0, 0, sumNum, res);
		return res;
	}
	void backtracking(vector<int> candidates, int target, int index, int sum, vector<int> sumNum, vector<vector<int>> &res) {
		if (sum == target)
		{
			res.push_back(sumNum);
			return;
		}
		else
		{
			for (int i = index; i < candidates.size(); i++)
			{
				if (sum + candidates[i]>target)//已经排序，可以剪枝
				{
					break;
				}
				sumNum.push_back(candidates[i]);
				backtracking(candidates, target, i, sum + candidates[i], sumNum, res);
				sumNum.pop_back();
			}
		}
	}
};
```

