---
title: 216. Combination Sum III
date: 2017-08-30 23:03:47
tags: LeetCode
categories: LeetCode
---

Find all possible combinations of k numbers that add up to a number n, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.

Example 1:
Input: k = 3, n = 7
Output:
[[1,2,4]]

Example 2:
Input: k = 3, n = 9
Output:
[[1,2,6], [1,3,5], [2,3,4]]

<!--more-->

## 题意：

　　找到所有可能的k个数字的组合，其总和为n，因为只能使用从1到9的数字，并且每个组合应该是唯一的数字集合。

## 思路：

　　典型的递归回溯题，需要找出所有的k个数的和等于n的组合，所以要求出所有情况，并且组合中没有重复元素，结果集中k个数的集合也是唯一的。

```c++
class Solution {
	vector<vector<int>> res;//存放k个数的所有组合
public:
	vector<vector<int>> combinationSum3(int k, int n) {
		vector<int> sumNum;
	    backtracking(n, k, 1,sumNum);
		return res;
	}
	void backtracking(int target, int counts, int index, vector<int> sumNum) {
		if (target==0)
		{
			if (counts==0)//满足这两个if条件才能放入结果集
			{
				res.push_back(sumNum);
				return;
			}
			
		}
		else
		{
			for (int i = index; i < 10; i++)//i是1到9，所以直接设定上界为10
			{
				sumNum.push_back(i);
				backtracking(target-i, counts-1,i + 1, sumNum);//因为子数组不存在重复元素，所以index递增1
				sumNum.pop_back();//递归回溯把之前进入子数组集的元素弹出
			}
		}
	}
};
```

