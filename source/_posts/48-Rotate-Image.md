---
title: 48. Rotate Image
date: 2017-07-21 22:04:41
tags: LeetCode
categories: LeetCode
---

# 48. Rotate Image

You are given an n x n 2D matrix representing an image.
Rotate the image by 90 degrees (clockwise).

Follow up:
　Could you do this in-place?

<!--more-->

## 题意：

　　　给定一个表示图像的n×n矩阵。将图像旋转90度（顺时针方向）。在常量空间复杂度下实现。

## 思路：

　　把n*n的矩阵按顺时针方向旋转90度，第一行变为最后一列，第二行变为倒数第二列。。。。。依次类推，所以旋转规律就是先按对角线反转，把除了对角线外的所有元素的从行变到相应的列，这样对称翻转后行原来相对顺序到列会成为逆序，所以按行反转就可得到旋转矩阵。  代码的主要难点就是要弄清楚行列坐标的控制条件。

　　**例子：**

```c++

 // 顺时针反转
 // 先进上下中轴进行行行交换, 再进行关于左上到右下对角线对称轴进行交换
 * 1 2 3     7 8 9     7 4 1
 * 4 5 6  => 4 5 6  => 8 5 2
 * 7 8 9     1 2 3     9 6 3
void rotate(vector<vector<int> > &matrix) {
    reverse(matrix.begin(), matrix.end());
    for (int i = 0; i < matrix.size(); ++i) {
        for (int j = i + 1; j < matrix[i].size(); ++j)
            swap(matrix[i][j], matrix[j][i]);
    }
}

 // 逆时针反转
 // 先进左右中轴进行列列交换, 再进行关于左上到右下对角线对称轴进行交换
 * 1 2 3     3 2 1     3 6 9
 * 4 5 6  => 6 5 4  => 2 5 8
 * 7 8 9     9 8 7     1 4 7
 void anti_rotate(vector<vector<int> > &matrix) {
    for (auto vi : matrix) reverse(vi.begin(), vi.end());
    for (int i = 0; i < matrix.size(); ++i) {
        for (int j = i + 1; j < matrix[i].size(); ++j)
            swap(matrix[i][j], matrix[j][i]);
    }
}
```

```c++
class Solution {
public:
	void rotate(vector<vector<int>>& matrix) {
		int n = matrix.size();
		for (int i =0;i<n;i++)//先进行关于左下到右上的对角线对称轴进行交换
		{
			for (int j = 0; j < n-i-1; j++) {
				swap(matrix[i][j], matrix[n - 1 - j][n - 1 - i]);
			}
		}
		int mid = n / 2;
		for (int i = 0; i < mid; i++)//再进行关于上下行中轴进行交换
		{
			for (int j = 0; j < n; j++) {
				swap(matrix[i][j], matrix[n - 1 - i][j]);
			}
		}
	}
};

```

---------------------------------------------------
#### 　Java Code
```Java
class Solution {
    public void rotate(int[][] matrix) {
        if (null == matrix) {
            return;
        }

        int row = matrix.length;
        if (row == 0) {
            return;
        }
        int col = matrix[0].length;

        for (int i = 0;i < row;i++) {
            for (int j = 0;j < i;j++) {
                int tmp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = tmp;
            }
        }

        for (int j = 0;j < col / 2;j++) {
            for (int i = 0;i < row;i++) {
                int tmp = matrix[i][j];
                matrix[i][j] = matrix[i][col - j - 1];
                matrix[i][col - j - 1] = tmp;
            }
        }
    }


}
```
