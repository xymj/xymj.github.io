---
title: 54. Spiral Matrix
date: 2017-07-25 21:12:58
tags: LeetCode
categories: LeetCode
---

# 54. Spiral Matrix

Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.

For example,
Given the following matrix:
[
　　[ 1, 2, 3 ],
　　[ 4, 5, 6 ],
　　[ 7, 8, 9 ]
]
You should return [1,2,3,6,9,8,7,4,5].

<!--more-->

## 题意：

　　给定一个m×n元素的矩阵（m行，n列），以螺旋顺序返回矩阵的所有元素（螺旋是顺时针方向）。

## 思路：

### 　方法一：

　　分四个不同的方向分别读取数组的值，重点是控制横坐标和纵坐标的变化，但是使用一个标记矩阵来记录已经访问过的元素。

```c++
class Solution {
public:
	vector<int> spiralOrder(vector<vector<int>>& matrix) {
		vector<int> res;
		int m = matrix.size();
		if (m==0)
		{
			return res;
		}
		int n = matrix[0].size();
		if (n==0)
		{
			return res;
		}
		vector<vector<int>> flag(m, vector<int>(n, 0));//标记矩阵
		int i = 0, j = 0;
		while (i<m&&j<n&& !flag[i][j])//只控制了i<m&&j<n条件，没能更精确控制行列坐标，所以借助标记数组标记已经访问过的元素，防止出错
		{
           //up
			while (j < n && !flag[i][j])
			{
				res.push_back(matrix[i][j]);
				flag[i][j] = 1;
				j++;
			}
			j--;
			i++;
          //right
			while (i<m&&!flag[i][j])
			{
				res.push_back(matrix[i][j]);
				flag[i][j] = 1;
				i++;
			}
			i--;
			j--;
          //down
			while (j >= 0 && !flag[i][j])
			{
				res.push_back(matrix[i][j]);
				flag[i][j] = 1;
				j--;
			}
			j++;
			i--;
          //left
			while (i >=0 && !flag[i][j])
			{
				res.push_back(matrix[i][j]);
				flag[i][j] = 1;
				i--;
			}
			i++;
			j++;
		}
		return res;
	}
};
```

### 　方法二：

　　也是分四个不同的方向分别读取数组的值，但是此方法控制横坐标和纵坐标的变化非常精细，可以省去标记矩阵来记录已经访问过的元素，完成螺旋的遍历,思路比方法一清晰，更好实现，代码简洁。

```c++
class Solution {
public:
	vector<int> spiralOrder(vector<vector<int>>& matrix) {
		int m = matrix.size();
		if (m == 0)
		{
			return {};
		}
		int n = matrix[0].size();
		if (n == 0)
		{
			return {};
		}
		vector<int> res(m*n);
		int l = 0, r = n - 1, u = 0, d = m - 1, k = 0;
		while (1)
		{
			// up
			for (int col = l;col<=r;col++)
			{
				res[k++] = matrix[l][col];
			}
			if(++u>d) break;
			//right
			for (int row = u; row <= d; row++)
			{
				res[k++] = matrix[row][r];
			}
			if (--r < l) break;
			//down
			for (int col = r; col >=l; col--)
			{
				res[k++] = matrix[d][col];
			}
			if (--d < u) break;
			//left
			for (int row = d; row >= u; row--)
			{
				res[k++] = matrix[row][l];
			}
			if (++l > r) break;
		}
		return res;
	}
};
```


---------------------------------------------------
#### 　Java Code
```Java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> res = new ArrayList();
        if (null == matrix) {
            return res;
        }

        int m = matrix.length;
        if (m == 0) {
            return res;
        }
        int n = matrix[0].length;

        int[][] flag = new int[m][n];
        int i = 0;
        int j = 0;
        while (i < m && j < n && flag[i][j] == 0) {
            while (j < n && flag[i][j] == 0) {
                res.add(matrix[i][j]);
                flag[i][j] = 1;
                j++;
            }
            j--;
            i++;

            while (i < m && flag[i][j] == 0) {
                res.add(matrix[i][j]);
                flag[i][j] = 1;
                i++;
            }
            i--;
            j--;


            while (j >= 0 && flag[i][j] == 0) {
                res.add(matrix[i][j]);
                flag[i][j] = 1;
                j--;
            }
            j++;
            i--;

            while (i >= 0 && flag[i][j] == 0) {
                res.add(matrix[i][j]);
                flag[i][j] = 1;
                i--;
            }
            i++;
            j++;
        }

        return res;
    }
}
```
