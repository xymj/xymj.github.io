---
title: 79. Word Search
date: 2017-08-07 21:58:55
tags: LeetCode
categories: LeetCode
---



# 79. Word Search

 Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

For example,
Given board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

word = "ABCCED", -> returns true,
word = "SEE", -> returns true,
word = "ABCB", -> returns false.

<!--more-->

## 题意：

　　给定一个2D面板和一个单词，查找这个单词是否存在于面板中。
　　单词可以由相邻单元格的字母构成，相邻的单元格是水平或垂直相邻的单元格。同一个字母单元不能超过一次使用。

## 思路：

### 　方法一：

　　也是典型的递归回溯，深度优先遍历的题目，但是因为同一个字母单元不能超过一次使用，所以要有标记矩阵标记此元素是否可以访问，从上下左右四个方向进行DFS。需要注意的就是访问一个字母后visited标识1，当DFS调用返回后，如果还没有找完，应该让visited置0，并返回false。
　　**注意理解的带返回值的终止递归回溯算法。**

```c++
/****************************************************************************************************************
下面递归回溯算法思路清晰，但是效率不是很高

You are here!  Your runtime beats 7.52% of cpp submissions.
87 / 87 test cases passed.
Status: Accepted
Runtime: 309 ms
****************************************************************************************************************/
class Solution {
public:
	bool exist(vector<vector<char>>& board, string word) {
		if (word.size() == 0)
			return true;
		int rowLen = board.size();
		int colLen = board[0].size();
		if (rowLen == 0 || colLen == 0)
			return false;
		vector<vector<int>> flag(rowLen, vector<int>(colLen, 0));
		//string tempStr = "";
		bool res = false;
		for (int i = 0; i < rowLen; i++)
		{
			for (int j = 0; j < colLen; j++)
			{
				if (board[i][j] == word[0] && !res)
				{
					if (backtrcaking(board, flag, word, 0, i, j))
						return true;
				}
			}
		}
		return false;

	}
	bool backtrcaking(vector<vector<char>>& board, vector<vector<int>> visited, string word, int wordIndex, int rowIndex, int colIndex) {
		if (wordIndex == word.size()-1)
		{
			return true;
		}
		int rowLen = board.size();
		int colLen = board[0].size();
		visited[rowIndex][colIndex] = 1;
		if (colIndex + 1 < colLen&&!visited[rowIndex][colIndex+1] &&word[wordIndex+1] == board[rowIndex][colIndex+1] )
		{
			if (backtrcaking(board, visited, word, wordIndex + 1, rowIndex, colIndex + 1))
			{
				return true;
			}
		}
		if (rowIndex + 1 < rowLen && !visited[rowIndex+1][colIndex] && word[wordIndex + 1] == board[rowIndex+1][colIndex])
		{
			if (backtrcaking(board, visited, word, wordIndex + 1, rowIndex + 1, colIndex))
			{
				return true;
			}
		}
		if (colIndex - 1 >= 0  && !visited[rowIndex][colIndex -1] && word[wordIndex + 1] == board[rowIndex][colIndex - 1])
		{
			if (backtrcaking(board, visited, word, wordIndex + 1, rowIndex, colIndex - 1))
			{
				return true;
			}
		}

		if (rowIndex - 1 >= 0 && !visited[rowIndex-1][colIndex] && word[wordIndex + 1] == board[rowIndex-1][colIndex])
		{
			if (backtrcaking(board, visited, word, wordIndex + 1, rowIndex - 1, colIndex))
			{
				return true;
			}
		}
		visited[rowIndex][colIndex] = 0;
		return false;

	}
};
```

### 　方法二：

　　递归回溯法，此法一是省去标记数组，减少空间复杂度，二是递归回溯函数内部递归结束条件控制的很好，所以能提高效率，值得学习的好方法。

```c++
class Solution {
public:
	bool exist(vector<vector<char>>& board, string word) {
		if (word.size() == 0)
			return true;
		int rowLen = board.size();
		int colLen = board[0].size();
		if (rowLen == 0 || colLen == 0)
			return false;
		for (int i = 0; i < rowLen; i++)
		{
			for (int j = 0; j < colLen; j++)
			{
				if (board[i][j] == word[0])//第一个单词元素满足，才能进行下面的深度优先遍历
				{
					if (backtrcaking(board, word, 0, i, j))//带返回值的回溯递归函数，利用返回值控制循环和递归
						return true;
				}
			}
		}
		return false;
	}
	bool backtrcaking(vector<vector<char>>& board,  string word, int wordIndex, int rowIndex, int colIndex) {
		if (wordIndex == word.size())
		{
			return true;
		}
		int rowLen = board.size();
		int colLen = board[0].size();
		if (rowIndex>=rowLen||rowIndex<0||colIndex>=colLen||colIndex<0)//此处优先判断索引条件，减少递归层次
		{
			return false;
		}
		char c = board[rowIndex][colIndex];
		if (c ==word[wordIndex])//此处优先比较面板元素值是否和单词元素相等，避免下面递归操作，减少循环递归层次
		{
			board[rowIndex][colIndex] = '#';//通过把遍历过的元素设置为‘#’，来避免使用标记数组
			if (backtrcaking(board, word, wordIndex + 1, rowIndex, colIndex + 1))
			{
				return true;
			}
			if (backtrcaking(board, word, wordIndex + 1, rowIndex + 1, colIndex))
			{
				return true;
			}
			if (backtrcaking(board, word, wordIndex + 1, rowIndex, colIndex - 1))
			{
				return true;
			}
			if (backtrcaking(board, word, wordIndex + 1, rowIndex - 1, colIndex))
			{
				return true;
			}
			board[rowIndex][colIndex] = c;
		}
		return false;
	}
};
```

