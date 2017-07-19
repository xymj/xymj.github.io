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

　　首先创建三个二维素组，rowIsValid\[ i ][ n ]记录第 i 行，第n（1~9）个值已经出现，colIsValid\[ j ][ n ]记录第 j 列，第n（1~9）个值已经出现，subboardIsValid\[ b ][ n ]记录第 b 个小九宫格，第n（1~9）个值已经出现。
　　然后遍历九宫格面板，依次标记上述三个数组，来进行设置已经出现1~9的值的标志位。
　　当标记数组填充完毕后，进行递归搜索，从0到80，即九宫格的81个元素，进行递归填充，每次填充元素的时候都要从1到9九个数字进行选取判断，如果三个标记数组中都没有出现此元素，则可以选取此元素，然后标记 此元素在三个标记数组中出现，最后递归填充下一个元素，如果成功不进行回溯，一直填充下去，如果不成功，要进行回溯，并且清除标记数组中的值，重新选取元素值。

```c++
class Solution {
private:
	int rowIsValid[9][10] = {{0}};//行元素标记数组
	int colIsValid[9][10] = {{0}};//列元素标记数组
	int subboardIsValid[9][10] = {{0}};//小九宫格块元素标记数组
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
		solve(board, 0);//三个标记数组填充完成，进行填充空白格
	}
	bool solve(vector<vector<char>>& board, int index)//递归回溯，填充1~9数字元素
	{
		if (index>80)
		{
			return true;
		}
		int row = index / 9;
		int col = index - 9*row;
		if (board[row][col]!='.')//单元格不为空，直接进行填充下一个单元格
		{
			return solve(board, index + 1);
		}
		for (int val =1;val<10;val++)
		{
			if (isValid(row,col,val))//判断填充空白格的元素是否有效
			{
				board[row][col] = val+'0';
				fill(row, col, val);//空白格填充后完成三个标记数组的标记
				if (solve(board,index+1))//回溯点，下一个元素填充满足的话向下层递归，不满足向上层回溯
				{
					return true;
				}
				clear(row, col, val);//回溯空白格填充不满足条件，清除三个标记数组的标记
			}
		}
		board[row][col] = '.';
		return false;
	}
	void fill(int row, int col, int val)//填充三个标记数组标记
	{
		rowIsValid[row][val] = 1;
		colIsValid[col][val] = 1;
		subboardIsValid[(row / 3) * 3 + col / 3][val] = 1;
	}
	void clear(int row, int col, int val)//清除三个标记数组标记
	{
		rowIsValid[row][val] = 0;
		colIsValid[col][val] = 0;
		subboardIsValid[(row / 3) * 3 + col / 3][val] = 0;
	}
	bool isValid(int row, int col, int val)//判读元素填充后是否是可行元素
	{
		if (rowIsValid[row][val] == 0 && colIsValid[col][val] == 0 && subboardIsValid[(row / 3) * 3 + col / 3][val] == 0)
		{
			return true;
        }
		return false;
	}
};
```

