---
title: 73. Set Matrix Zeroes
date: 2017-08-04 21:05:12
tags: LeetCode
categories: LeetCode
---

# 73. Set Matrix Zeroes

Given a m x n matrix, if an element is 0, set its entire row and column to 0. Do it in place.

Follow up:
Did you use extra space?
A straight forward solution using O(mn) space is probably a bad idea.
A simple improvement uses O(m + n) space, but still not the best solution.
Could you devise a constant space solution?

<!--more-->

## 题意：

　　给定一个m×n矩阵，如果一个元素为0，则将整个行和列设置为0。在原内存空间上完成。

## 思路：

　　利用第一行和第一列做为标志行，先进行遍历，如果存在0，设置标志，最后第一行第一列全部置为0，然后遍历除去第一行和第一列的数组元素，当元素为0时，那么这个元素所在的第一行和第一列最后肯定也置为0，所以直接把元素所在行和列的第一个元素置为0，因为遍历是从第二行和第二列开始的，所以第一行的置0的状态并不影响最后转换的数组状态。
　　**思路的主要点**就是利用第一行和第一列做为0元素的标志,重点就是一直到最后才能改变第一行和第一列是否全为0的状态。

```c++
class Solution {
public:
	void setZeroes(vector<vector<int>>& matrix) {
		int m = matrix.size();
		if (m == 0)
		{
			return;
		}
		int n = matrix[0].size();
		if (n == 0)
		{
			return;
		}
		int firstRowFlag = 0, firstColFlag = 0;
		for (int i =0;i<m;i++)
		{
			if (matrix[i][0]==0)
			{
				firstColFlag = 1;
				break;
			}
		}
		for (int j = 0;j<n;j++)
		{
			if (matrix[0][j]==0)
			{
				firstRowFlag = 1;
				break;
			}
		}
		for (int i = 1; i < m; i++)
		{
			for (int j = 1; j < n; j++)
			{
				if (matrix[i][j]==0)
				{
					matrix[i][0] = 0;
					matrix[0][j] = 0;
				}
			}
		}
		//下面的写法是根据首行和首列的元素为0然后把对应的行列置为0，这是大错特错，因为首行首列的第一个元素nums[0][0]==0可能为零，这样下面方法根据第一个元素把首行先置为0，会造成首行不该为零的元素置为0，当再遍历列是根据第一行就会出错。
		//所以应该根据遍历[1-m][1-n]中元素，然后去看此元素对应的第一行或第一列是否为0，为零的话直接把此元素置为0，因为第一行或第一列是为0表明以前此元素所在的行或者列出现过0,此元素应该置为0。
		/*for (int i = 0; i < m; i++)
		{
			if (matrix[i][0] == 0)
			{
				for (int j = 1; j < n; j++)
				{
					matrix[i][j] = 0;
				}
			}
		}
		printf("********************\n");
		for (auto r : matrix)
		{
			for (auto c : r)
			{
				printf("%4d", c);
			}
			printf("\n");
		}
		for (int j = 0; j < n; j++)
		{
			if (matrix[0][j] == 0)
			{
				for (int i = 1; i < m; i++)
				{
					matrix[i][j] = 0;
				}
			}
		}
		printf("********************\n");
		for (auto r : matrix)
		{
			for (auto c : r)
			{
				printf("%4d", c);
			}
			printf("\n");
		}*/

		//正确写法，根据遍历的元素确定元素是否为0
		for (int i = 1; i < m; i++)
		{
			for (int j = 1; j < n; j++)
			{
				if (matrix[i][0] == 0||matrix[0][j]==0)
				{
					matrix[i][j] = 0;
				}
			}
		}
		if (firstRowFlag)
		{
			for (int j = 0; j < n; j++)
			{
				matrix[0][j] = 0;
			}
		}
		if (firstColFlag)
		{
			for (int i = 0; i < m; i++)
			{
				matrix[i][0] = 0;
			}
		}
	}
};
```

