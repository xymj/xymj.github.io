---
title: 36. Valid Sudoku
date: 2017-07-18 23:14:08
tags: LeetCode
categories: LeetCode
---

# 36. Valid Sudoku

Determine if a Sudoku is valid, according to : [Sudoku Puzzles - The Rules](http://sudoku.com.au/TheRules.aspx).
The Sudoku board could be partially filled, where empty cells are filled with the character '.'.

![36-Sudoku](/images/36-Sudoku-by-L2G-20050714.svg.png)`
A partially filled sudoku which is valid.

Note :
　　A valid Sudoku board(partially filled) is not necessarily solvable.Only the filled cells need to be validated.

<!--more-->

## 题意：

　　确定给出的九宫格是否一个有效的数独，数独谜题的根据：[规则](http://sudoku.com.au/TheRules.aspx)。数独板可能是部分元素填充，没填充的元素用字符“.”代替。
　　**注意**　：一个有效的数独板（部分填充）不一定是可解的，只需要验证填充的单元格是否有效。　　

## 思路：

　　注意到题目中说的,只要当前已经填充的数字是合法的就可以,不一定要这个数独是有解。因此只需要判断9*9网格的每一行、每一列、9个小九宫格是否合法。即如果在每一行、每一列、每个9个小九宫格内，某个数字重复出现了，当前数独就是不合法的。 其中一种简单方法就是把行、列、九宫格、分三个两重循环来分别判断是否合法。
　　其实只需要一个两重循环即可，在双重循环时，我们默认了第一重循环表示矩阵的行、第二重循环表示矩阵的列。可以换一种思路：

1. 在检测行是否合法时，i 表示矩阵的行，j 表示矩阵的列。
2. 检测列是否合法时，i 表示矩阵的列，j 表示矩阵的行。
3. 检测九宫格是否合法时，i 表示九宫格的标号，j 表示九宫格里的每个元素（只是我们需要根据i、j定位相应的元素到原来的矩阵：第 i 个九宫格里面的第 j 个元素在原矩阵的第 3*(i/3) + j/3 行，第 3\*(i%3) + j%3）列，“/” 表示整数除法）。

```c++
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        vector<int> rowValid(10, 0);
    	vector<int> colValid(10, 0);
    	vector<int> boardValid(10, 0);
    	for (int i=0;i<9;i++)
    	{
    		for (int j =0;j<9;j++)
    		{
    			if (board[i][j]!='.'&&board[i][j] - '0' >= 1&& board[i][j] - '0' <= 9)
    			{
    				//判断行是否存在相同的元素
    				if (rowValid[board[i][j] - '0'] == 1)
    				{
    					return false;
    				}
    				else
    					rowValid[board[i][j] - '0'] = 1;
    			}
    			if (board[j][i]!='.'&&board[j][i] - '0' >= 1&& board[j][i] - '0' <= 9)
    			{
    				//判断列是否存在相同的元素
    				if (colValid[board[j][i]-'0'] == 1)
    				{
    					return false;
    				}
    				else
    					colValid[board[j][i]-'0'] = 1;
    			}

    			if (board[i/3*3 + j/3][(i%3)*3 + j%3]!='.'&&board[i/3*3 + j/3][(i%3)*3 + j%3] - '0' >= 1&& board[i/3*3 + j/3][(i%3)*3 + j%3] - '0' <= 9)
    			{
    				//判断区域是否存在相同的元素,i表示第几个区域，j表示区域内的第几个元素,根据i和j确定在board中的元素
    				if (boardValid[board[i/3*3 + j/3][(i%3)*3 + j%3]-'0'] == 1)
    				{
    					return false;
    				}
    				else
    					boardValid[board[i/3*3 + j/3][(i%3)*3 + j%3]-'0'] = 1;
    			}
    		}
    		for (int n =0;n<10;n++)
    		{
    			rowValid[n] = 0;
    			colValid[n] = 0;
    			boardValid[n] = 0;

    		}
    	}
    	return true;
    }
};
```



---------------------------------------------------
#### 　Java Code：
```Java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        if (null == board) {
            return false;
        }
        for (int i = 0; i < 9; i++) {
            int[] rowCheck = new int[10];
            int[] colCheck = new int[10];
            int[] boxCheck = new int[10];
            for (int j = 0; j < 9; j++) {
                if (board[i][j] != '.') {
                    int rowNum = board[i][j] - '0';
                    if (rowNum >= 1 && rowNum <= 9) {
                        if (rowCheck[rowNum] == 1) {
                            System.out.println("row");
                            return false;
                        } else {
                            rowCheck[rowNum] = 1;
                        }
                    }
                }
                if (board[j][i] != '.') {
                    int colNum = board[j][i] - '0';
                    if (colNum >= 1 && colNum <= 9) {
                        if (colCheck[colNum] == 1) {
                            System.out.println("col");
                            return false;
                        } else {
                            colCheck[colNum] = 1;
                        }
                    }
                }
                if (board[(i / 3) * 3 + j / 3][(i % 3) * 3 + j % 3] != '.') {
                    int boxNum = board[(i / 3) * 3 + j / 3][(i % 3) * 3 + j % 3] - '0';
                    if (boxNum >= 1 && boxNum <= 9) {
                        if (boxCheck[boxNum] == 1) {
                            return false;
                        } else {
                            boxCheck[boxNum] = 1;
                        }
                    }
                }
            }
        }
        return true;
    }
```
