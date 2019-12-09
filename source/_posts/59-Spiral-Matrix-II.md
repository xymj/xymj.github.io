---
title: 59. Spiral Matrix II
date: 2017-07-26 22:24:08
tags: LeetCode
categories: LeetCode
---

# 59. Spiral Matrix II

Given an integer n, generate a square matrix filled with elements from 1 to n2 in spiral order.

For example,
Given n = 3,
You should return the following matrix:

[
　[ 1, 2, 3 ],
　[ 8, 9, 4 ],
　[ 7, 6, 5 ]
]

<!--more-->

## 题意：

　　给定一个整数n，生成一个以1到n的平方根为螺旋顺序元素的螺旋矩阵。

## 思路：

　　主要参照[54. Spiral Matrix](http://blog.taoaili999.cn/2017/07/25/54-Spiral-Matrix/)方法二。

```c++
class Solution {
public:
	vector<vector<int>> generateMatrix(int n) {
		vector<vector<int>> res(n,vector<int>(n, 0));
		int l = 0, r = n - 1, u = 0, d = n - 1, k = 1;
		while (1)
		{
			//up
			for (int col = l;col<=r;col++)
			{
				res[u][col] = k++;
			}
			if (++u>d) break;
			//right
			for (int row = u; row <= d; row++)
			{
				res[row][r] = k++;
			}
			if (--r < l) break;
			//down
			for (int col = r; col >= l; col--)
			{
				res[d][col] = k++;
			}
			if (--d < u) break;
			//left
			for (int row = d; row >= u; row--)
			{
				res[row][l] = k++;
			}
			if (++l > r) break;
		}
		return res;
	}
};
```


---------------------------------------------------
### 　Java Code
```Java
class Solution {
    public int[][] generateMatrix(int n) {
        if (n == 0) {
            return new int[0][0];
        }

        int[][] res = new int[n][n];
        int left = 0;
        int right = n - 1;
        int up = 0;
        int down = n - 1;
        int element = 1;

        while (true) {
            for (int col = left; col <= right;col++) {
                res[up][col] = element++;
            }
            up++;
            if (up > down) {
                break;
            }


            for (int row = up; row <= down;row++) {
                res[row][right] = element++;
            }
            right--;
            if (right < left) {
                break;
            }


            for (int col = right; col >= left;col--) {
                res[down][col] = element++;
            }
            down--;
            if (down < up) {
                break;
            }


            for (int row = down; row >= up;row--) {
                res[row][left] = element++;
            }
            left++;
            if (left > right) {
                break;
            }
        }

        return res;
    }
}
```
