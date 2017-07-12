---
title: 28. Implement strStr()
date: 2017-07-12 22:44:18
tags: LeetCode
categories: LeetCode
---

# 28. Implement strStr()

Implement strStr().
Returns the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

## 题意：

　　返回在haystack中第一次出现的needle的索引，如果不存在就返回-1。

<!--more-->

## 思路：

### 　方法一：

　　暴力解决，从左到右遍历haystack每一个元素，查找needle第一个字符出现的位置，然后截取和needle等长的子串，比较二者是否相等，相等则找到，不等继续遍历，如果到最后还没有找到，返回-1，此法效率低。

```c++
//You are here!   Your runtime beats 2.39% of cpp submissions.
//73 / 73 test cases passed.
//Status: Accepted
//Runtime: 36 ms
class Solution{
public:
	int strStr(string haystack, string needle) {
		int index = -1;
		if (needle.empty())
			return 0;
		if (haystack.empty())
			return index;
		size_t pos = haystack.find_first_of(needle[0]);
		while (pos != string::npos)
		{
			string tempNeedle = haystack.substr(pos, needle.size());
			if (tempNeedle == needle)
			{
				index = pos;
				break;
			}
			else {
				pos = haystack.find_first_of(needle[0], pos + 1);
			}
		}
		return index;
	}
};
```

### 　方法二：

　　利用KMP算法，通过求next数组，完成子串和目标串的匹配，[KMP算法详解](http://blog.csdn.net/v_july_v/article/details/7041827)。

```c++
//You are here!  Your runtime beats 13.13% of cpp submissions.
//73 / 73 test cases passed.
//Status: Accepted
//Runtime: 6 ms
class Solution {
public:
	//取主串第一次匹配模式串的位置
	int strStr(string haystack, string needle) {
		int haystackLen = haystack.size();
		int needleLen = needle.size();
		if (needleLen==0)
		{
			return 0;
		}
		vector<int> next(needleLen, 0);
		getNext(needle, next);
		int i = 0, j = 0;
		while (i<haystackLen&&j<needleLen)
		{
			if (j==-1||haystack[i]==needle[j])
			{
				i++;
				j++;
			}
			else
			{
				j = next[j];
			}
		}
		if (j == needleLen)
		{
			return i - j;
		}
		else
			return -1;
	}
	//得到KMP算法所需要的next[]数组
	void getNext(string p, vector<int> &next)
	{
		int  pLen = p.size();
		next[0] = -1;
		int k = -1;
		int j = 0;
		while (j<pLen-1)
		{
			if (k==-1||p[k]==p[j])
			{
				k++;
				j++;
				if (p[j] != p[k])
				{
					next[j] = k;
				}
				else
					next[j] = next[k];
			}
			else {
				k = next[k];
			}
		}
	}
};
```

### 　方法三：

　　利用C++的string中封装的find方法，[find方法详解](http://www.cplusplus.com/reference/string/string/find/)。

```c++
//You are here!  Your runtime beats 57.50% of cpp submissions.
//73 / 73 test cases passed.
//Status: Accepted
//Runtime: 3 ms
class Solution {
public:
	int strStr(string haystack, string needle) {
		string::size_type temp = haystack.find(needle);
		if (temp != std::string::npos)
			return int(temp);
		else
			return -1;
	}
};
```

