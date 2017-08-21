---
title: 118. Pascal's Triangle
date: 2017-08-18 23:07:35
tags: LeetCode
categories: LeetCode
---

# 118. Pascal's Triangle

Given numRows, generate the first numRows of Pascal's triangle.

For example, given numRows = 5,
Return

[
　　[1],
　　[1,1],
　　[1,2,1],
　　[1,3,3,1],
　　[1,4,6,4,1]
]

<!--more-->

## 题意：

　　给定一个行数numrows，产生相应行数的Pascal三角形。 

## 思路：

　　根据杨辉三角规律，通过上述例子也可以找到规律，从第三行开始，每行除了第一个和最后一个是1外，其他元素等于上一行对应两个元素的和，比如第 i 行 j 列（i > 1 &&  j > 0）,res\[i]\[j] = res\[ i - 1 ]\[ j ] + res\[i - 1][ j - 1 ]。

```c++
class Solution {
public:
	vector<vector<int>> generate(int numRows) {
		vector<vector<int>> res;
		for (int i =0;i<numRows;i++)
		{
			vector<int> temp(i+1);
			if (i==0)
			{
				temp[0] = 1;
				res.push_back(temp);
			}
			else if (i==1)
			{
				temp[0] = 1;
				temp[i] = 1;
				res.push_back(temp);
			}
			else
			{		
				temp[0] = 1;
				for (int j = 1;j<i;j++)
				{
					temp[j] = res[i - 1][j - 1] + res[i - 1][j];
				}
				temp[i] = 1;
				res.push_back(temp);
			}
		}
		return res;
	}
};
```

