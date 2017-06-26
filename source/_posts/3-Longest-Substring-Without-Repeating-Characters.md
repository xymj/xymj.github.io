---
title: 3. Longest Substring Without Repeating Characters
date: 2017-06-26 10:16:39
tags: LeetCode
categories: LeetCode
---

# 3. Longest Substring Without Repeating Characters 



Given a string, find the length of the **longest substring** without repeating characters.

**Examples:**

Given `"abcabcbb"`, the answer is `"abc"`, which the length is 3.

Given `"bbbbb"`, the answer is `"b"`, with the length of 1.

Given `"pwwkew"`, the answer is `"wke"`, with the length of 3. Note that the answer must be a **substring**, `"pwke"` is a *subsequence* and not a substring.

<!-- more -->

## 题意：

　　从一个字符串中查找子串，但是这个子串中不能包含重复的字符，并且要找出最长的不重字符复子串。

## 思路：

　　利用字符映射表来记录此字符之前（包括字符本身）有多少个元素，然后再利用一个元素来记录，此元素对应的之前重复的那个元素的之前的元素个数。



```C++
class Solution
{
public:
	int lengthOfLongestSubstring(string s)
	{
		//键存储的是字符，值存储的此字符之前（包含此字符自身）有几个元素。
		unordered_map<char, int> hash;
		int n = s.size();
		int len = 0;
		for (int i=0,j=0;j<n;j++)
		{
			if (hash.count(s[j]))
			{
				//i之前的包括i指向的元素一定包含重复元素，所以在此取i的值的时候要比较当前重复元素时出现在i前面还是后面
				//后面的话更新i的值，否则i值不变，例如abba
				i = max(hash[s[j]], i);
			}
			len = max(len, j - i + 1);
			hash[s[j]] = j+1;
		}
		return len;
	}
};
```

