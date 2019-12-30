---
title: 74. Search a 2D Matrix
date: 2017-08-04 21:17:49
tags: LeetCode
categories: LeetCode
---

# 74. Search a 2D Matrix

Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:
　　Integers in each row are sorted from left to right.
　　The first integer of each row is greater than the last integer of the previous row.

For example,
Consider the following matrix:
[
　　[1,   3,  5,  7],
　　[10, 11, 16, 20],    
　　[23, 30, 34, 50]
]
Given target = 3, return true.
<!--more-->

## 题意：

　　 编写一个有效的算法，在m×n矩阵中搜索一个值。这个矩阵具有以下属性：
　　　　每行中的整数从左到右排序。
　　　　每一行的第一个整数大于前一行的最后一个整数。

## 思路：

　　二分查找，因为是按行排序好的二维数组，所以直接把二维数组看做一维数组，一次二分查找搞定。
　　n * m 的矩阵m可以转化为一个一维数组 => matrix\[x]\[y] => a\[x * m + y]
　　一维数组转化为n * m的矩阵m => a[x] =>matrix\[x / m]\[x % m];

```c++
class Solution {
public:
	bool searchMatrix(vector<vector<int>>& matrix, int target) {
		int m = matrix.size();
		if (m == 0)
		{
			return false;
		}
		int n = matrix[0].size();
		if (n == 0)
		{
			return false;
		}
		int allNum = m*n;
		int l = 0, r = allNum - 1;
		while (l <= r)
		{
			int midCol = (l + r) / 2;
			if (target < matrix[midCol / n][midCol%n])//把二维数组转化为一维数组，这种找下标索引的（行索引==索引/列数，列索引==索引%列数）方法及其好，需记忆
			{
				r = midCol - 1;
			}
			else if (target > matrix[midCol / n][midCol%n])
			{
				l = midCol + 1;
			}
			else
				return true;
		}
		return false;
	}
};
```

---------------------------------------------------
### 　Java Code
```Java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (null == matrix) {
            return false;
        }
        int m = matrix.length;
        if (0 == m) {
            return false;
        }
        int n = matrix[0].length;
        if (0 == n) {
            return false;
        }

        int row = 0;
        while (row < m) {
            if (matrix[row][n - 1] >= target) {
                break;
            }
            row++;
        }
        if (row == m) {
            return false;
        }
        int left = 0;
        int right = n - 1;
        while (left <= right) {
            int mid = (left + right) / 2;
            int cur = matrix[row][mid];
            if (cur == target) {
                return true;
            } else if (cur < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return false;
    }
}
```
