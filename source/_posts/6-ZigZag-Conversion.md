---
title: 6. ZigZag Conversion
date: 2017-06-29 21:44:08
tags: LeetCode
categories: LeetCode
---

# 6. ZigZag Conversion 

The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility(易读的))

```
P   A   H   N
A P L S I I G
Y   I   R
```

```
"PAHNAPLSIIGYIR"
```

And then read line by line: `"PAHNAPLSIIGYIR"`

Write the code that will take a string and make this conversion given a number of rows:

```
string convert(string text, int nRows);
convert("PAYPALISHIRING", 3)
"PAHNAPLSIIGYIR"
```

<!-- more -->

## 题意：

　　把一个字符串根据给的的行数，按之字形模式进行输出，然后按照输出的之字形模式按行读取，返回一个新的字符串。

## 思路：

### 　方法一：

　　创建nRows个string数组，将s字符串按照0,1,2,...,nRows-2,nRows-1，然后nRows-2,...,2,1的顺序，组个把s串中的字符放入到各个string中。
　　具体来说，string数组下标先递增，直到nRows-1，则反向，直到0，再反向，……直到s字符串遍历完，再依次组合0~nRows-1个string元素。切记一点**当到nRows-1返回的时候要从nRows-2开始**，因为之字形返回最后一行数组s中字符元素，但是正向第一行是有的，所以从0开始。

```c++
class Solution {
public:
	string convert(string s, int numRows) {
		if (numRows == 1)
			return s;
		string res = "";
		int sLen = s.size();
		vector<string> val(numRows);
		int down = 0, up = numRows-2;//向下记录从0开始，向上记录从 numRows-2开始，
		int i = 0;
		while (i<sLen)
		{
			for (int j = down; j < numRows&&i<sLen;)
			{
				val[j++] += s[i++];
			}
			for (int j = up; j > 0&&i<sLen;)
			{
				val[j--] += s[i++];
			}
		}
		for (auto v : val)
		{
			res += v;
		}
		return res;
	}
};
```



### 　方法二：

　　创建nRows个int数组，将s字符串的索引按照0,1,2,...,nRows-2,nRows-1，然后nRows-2,...,2,1,0的顺序装入int输出中。
　　具体来说，数组下标先递增，直到nRows-1，则反向，直到0，再反向，……直到s字符索引全部装入各数组，再依次组合0~nRows-1个数组。切记一点**当到nRows-1返回的时候要从nRows-2开始记录索引**，因为之字形返回最后一行数组没有索引元素，但是正向第一行是有索引元素的，所以从0开始。

```c++
class Solution2 {
public:
	string convert(string s, int numRows) {
		if (numRows == 1)
			return s;
		string res = "";
		int sLen = s.size();
		vector<vector<int>> sIndex(numRows, vector<int>());
		int index = 0, flag = 0;
		for (int i = 0;i<sLen;i++)
		{
			index += flag;
			sIndex[index].push_back(i);
			if (index==numRows-1)
			{
				flag = -1;
			}
			else if(index==0)
			{
				flag = 1;
			}
		}
		for (int i =0;i<numRows;i++)
		{
			for (int j =0;j<sIndex[i].size();j++)
			{
				res += s[sIndex[i][j]];
			}
		}
		return res;
	}
};
```

