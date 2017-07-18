---
title: 37. Sudoku Solver
date: 2017-07-18 23:31:16
tags: LeetCode
categories: LeetCode
---

# 37. Sudoku Solver

Write a program to solve a Sudoku puzzle by filling the empty cells.
Empty cells are indicated by the character '.'.
You may assume that there will be only one unique solution.
![37-1-Sudoku](/images/37-1-Sudoku-by-L2G-20050714.svg.png)
A sudoku puzzle...
![37-2-Sudoku](/images/37-2-Sudoku-by-L2G-20050714_solution.svg.png)
...and its solution numbers marked in red.

<!--more-->

## 题意：

　　 编写一个程序，通过填充空单元格来解决数独难题。 空单元格用字符'.'表示。  可以假设仅有一种解决方法。具体填充实例看上图，规则参照[36-Valid-Sudoku](http://blog.taoaili999.cn/2017/07/18/36-Valid-Sudoku/)。 

## 思路：

　　思路。。。

```c++
class Solution {
public:
    void solveSudoku(vector<vector<char>>& board) {
        for (int i =0;i<9;i++)
		{
			for (int j = 0;j<9;j++)
			{
				if (board[i][j]!='.')
				{
					fill(i, j, board[i][j] - '0');
				}
			}
		}
		solve(board, 0);
	}
	bool solve(vector<vector<char>>& board, int index)
	{
		if (index>80)
		{
			return true;
		}
		int row = index / 9;
		int col = index - 9*row;
		if (board[row][col]!='.')
		{
			return solve(board, index + 1);
		}
		for (int val =1;val<10;val++)
		{
			if (isValid(row,col,val))
			{
				board[row][col] = val+'0';
				fill(row, col, val);
				if (solve(board,index+1))
				{
					return true;
				}
				clear(row, col, val);
			}
		}
		board[row][col] = '.';
		return false;
	}
	void fill(int row, int col, int val)
	{
		rowIsValid[row][val] = 1;
		colIsValid[col][val] = 1;
		subboardIsValid[(row / 3) * 3 + col / 3][val] = 1;
	}
	void clear(int row, int col, int val)
	{
		rowIsValid[row][val] = 0;
		colIsValid[col][val] = 0;
		subboardIsValid[(row / 3) * 3 + col / 3][val] = 0;
	}
	bool isValid(int row, int col, int val)
	{
		if (rowIsValid[row][val] == 0 && colIsValid[col][val] == 0 && subboardIsValid[(row / 3) * 3 + col / 3][val] == 0)
		{
			return true;
        }
		return false;

	}
private:
	int rowIsValid[9][10] = {{0}};
	int colIsValid[9][10] = {{0}};
	int subboardIsValid[9][10] = {{0}};
};
```

