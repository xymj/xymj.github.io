---
title: 64. Minimum Path Sum
date: 2017-07-27 22:26:20
tags: LeetCode
categories: LeetCode
---

# 64. Minimum Path Sum

Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

Note: You can only move either down or right at any point in time.

<!--more-->

## 题意：

　　给定一个充满非负数的m×n网格，从左上角到右下角找到一条路径，它沿着路径将所有数的总和最小化。

## 思路：

### 　方法一：

　　思路同[62. Unique Paths](http://blog.taoaili999.cn/2017/07/27/63-Unique-Paths-II/)利用动态规划，但是此题在通过动态转移方程计算的时候，就是每前进一步，就计算上一步的最小值，根据之前的最小值推断出当前最小值，即局部最优构造全局最优。但是网上的好处就是没有改变原数组的值，利用新的数组来存储最小值。

```c++
class Solution {
public:
	int minPathSum(vector<vector<int>>& grid) {
		int m = grid.size();
		if (m == 0)  return 0;
		int n = grid[0].size();
		if (n == 0)  return 0;
		for (int j =1;j<n;j++)
		{
			grid[0][j] += grid[0][j - 1];//累加第一行从左到右行走的路径值
		}
		for (int i =1;i<m;i++)
		{
			grid[i][0] += grid[i - 1][0];//累加第一列从上到下行走的路径值
		}
		for (int i = 1;i<m;i++)
		{
			for (int j = 1;j<n;j++)
			{
				grid[i][j] += min(grid[i - 1][j], grid[i][j - 1]);//状态转移方程，通过左边上上边路径值的最小值确定当前节点的路径最小值
			}
		}
		return grid[m - 1][n - 1];
	}
};
```

### 　方法一：

　　思路也是动态规划，但是与方法一不同点时好处就是利用新的一维数组来存储要求元素点从上边到来的路径值，没有改变原数组的值。

```c++
class Solution {
public:
	int minPathSum(vector<vector<int>>& grid) {
		int m = grid.size();
		if (m == 0)  return 0;
		int n = grid[0].size();
		if (n == 0)  return 0;
		vector<int> res(n, INT_MAX);
		res[0] = 0;
		for (int i =0;i<m;i++)
		{
			res[0] += grid[i][0];
			for (int j =1;j<n;j++)
			{
				res[j] = min(res[j - 1], res[j]) + grid[i][j];//res[j-1]存储从左边到当前元素的路径值，res[j]存储的是从上边到当前元素的路径值
			}
		}
		return res[n-1];
	}
};
```

