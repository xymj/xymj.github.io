---
title: 63. Unique Paths II
date: 2017-07-27 22:02:54
tags: LeetCode
categories: LeetCode
---

# 63. Unique Paths II

Follow up for "Unique Paths":
Now consider if some obstacles are added to the grids. How many unique paths would there be?
An obstacle and empty space is marked as `1` and `0` respectively in the grid.

For example,
There is one obstacle in the middle of a 3x3 grid as illustrated below.

```
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
```

The total number of unique paths is `2`.

**Note:** *m* and *n* will be at most 100.

<!--more-->

## 题意：

　　[62. Unique Paths](http://blog.taoaili999.cn/2017/07/27/63-Unique-Paths-II/)变形,现在考虑在网格中添加一些障碍物。会有多少独特的路径？
　　在方格中，障碍物和空区分别标有‘1’和‘0’。

## 思路：

　　思路同62. Unique Paths](http://blog.taoaili999.cn/2017/07/27/63-Unique-Paths-II/),都是利用动态规划，不同点是要考虑出现障碍物，把有障碍物(obstacles)的地方的路径数直接设置为0，特别注意数据类型界限问题，越界会获取不到正确的数据而造成未知错误。根据障碍物出现的位置，设置辅助矩阵元素值为零主要分三种情况：
　　　　1）在矩阵第一个元素，即obstacleGrid\[0]\[0]，这样不能到达，所以直接返回路径数为0。
　　　　2）在矩阵第一行的某个元素，即obstacleGrid\[0]\[j]，这样第一行j列之后不能到达，第一行j列之后辅助矩阵元素直接设0。
　　　　3）在矩阵第一列的某个元素，即obstacleGrid\[i]\[0]，这样第一列i行之后不能到达，第一列i行之后辅助矩阵元素直接设0。
　　　　4）在矩阵中某个元素，即obstacleGrid\[i]\[j](i > 0,j > 0)，辅助矩阵元素直接设0。

```C++
class Solution {
public:
	int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
		int m = obstacleGrid.size();
		if (m==0)
		{
			return 0;
		}
		int n = obstacleGrid[0].size();
		if (n == 0) return 0;
		if (obstacleGrid[0][0])
		{
			return 0;
		}
		vector<vector<double>> dp(m, vector<double>(n, 0));//这个地方如果用int类型，中间值相加的时候有可能越界，造成错误RunTime error，所以用更大的数据类型double，可以测试通过
		for (int i = 0;i<m;i++)
		{
			if (!obstacleGrid[i][0])
			{
				dp[i][0] = 1;
			}
			else
				break;
		}
		for (int j = 1; j< n; j++)
		{
			if (!obstacleGrid[0][j])
			{
				dp[0][j] = 1;
			}
			else
				break;
		}
		for (int i = 1; i < m; i++)
		{
			for (int j = 1; j < n; j++)
			{
				if (!obstacleGrid[i][j])
				{
					dp[i][j] = dp[i][j - 1] + dp[i - 1][j];
				}
				else
					continue;
			}
		}
		return dp[m - 1][n - 1];
	}
};
```