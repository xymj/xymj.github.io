---
title: 120. Triangle
date: 2017-08-19 13:48:11
tags: LeetCode
categories: LeetCode
---

# 120. Triangle

Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

```c++
For example, given the following triangle
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
The minimum path sum from top to bottom is 11 (i.e., 2 + 3 + 5 + 1 = 11).
```

Note:
Bonus point if you are able to do this using only O(n) extra space, where n is the total number of rows in the triangle. 

<!--more-->

## 题意：

　　给定一个三角形，找到从上到下的最小路径求和。每一步你可以移动到下面一行**相邻的数字****，**特别注意是相邻的数字**。
　　只用O（n）额外的空间来做，其中n是三角形中的总行数。

## 思路：

　　 从三角形的底部往顶部走进行遍历，一次查找最小元素，这样当查找本行的最小和时，只需与下一行相邻元素相加的和进行比较求出最小和。  
　　这道题最主要需要学的地方就是换角度思考问题，把上到的下的查找转换为下到上的查找，从而降低代码逻辑难度。

```C++
class Solution {
public:
	int minimumTotal(vector<vector<int>>& triangle) {
	
		int m = triangle.size();
		if (m == 0) return 0;
		int n = triangle[0].size();
		if (n == 0) return 0;
		
		vector<int> minSum(triangle[m - 1]);
		for (int i = m - 2; i >= 0; i--)
		{
			for (int j =0;j<triangle[i].size();j++)
			{
				minSum[j] = min(minSum[j], minSum[j + 1]) + triangle[i][j];
			}
		}
		return minSum[0];

	}
};
```

　　下面是从上往下遍历三角形的过程中，求出当前行每个元素与上层元素相加的最小和，要考虑的边界问题相对较多，代码繁琐。

```c++
class Solution {
public:
	int minimumTotal(vector<vector<int>>& triangle) {
		/*[
			[2],
			[3, 4],
			[6, 5, 7],
			[4, 1, 8, 3]
		]*/
		int m = triangle.size();
		if (m == 0) return 0;
		int n = triangle[0].size();
		if (n == 0) return 0;
		if (m == 1 && n == 1)
			return triangle[0][0];
		int sum = INT_MAX;
		for (int i =1;i<m;i++)
		{
			int col = triangle[i].size();
			for (int j = 0;j<col;j++)
			{
				if (j==0)
				{
					triangle[i][j] += triangle[i - 1][j];
					if (i==m-1)
					{
						sum = min(sum, triangle[i][j]);
					}
				}
				else if (j==col - 1)
				{
					triangle[i][j] += triangle[i - 1][j - 1];
					if (i == m - 1)
					{
						sum = min(sum, triangle[i][j]);
					}
				}
				else
				{
					triangle[i][j] += min(triangle[i - 1][j], triangle[i - 1][j - 1]);
					if (i == m - 1)
					{
						sum = min(sum, triangle[i][j]);
					}
				}
			}
		}
		return sum;
	}
};
```

