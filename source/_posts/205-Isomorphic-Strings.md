---
title: 205. Isomorphic Strings
date: 2017-08-28 22:31:53
tags: LeetCode
categories: LeetCode
---

Given two strings s and t, determine if they are isomorphic.
Two strings are isomorphic if the characters in s can be replaced to get t.

All occurrences of a character must be replaced with another character while preserving the order of characters.No two characters may map to the same character but a character may map to itself.

For example,
Given "egg", "add", return true.
Given "foo", "bar", return false.
Given "paper", "title", return true.
Note:
You may assume both s and t have the same length.

<!--more-->

## 题意：

　　给定两个字符串S和T，确定两个字符串是否是同构的。
　　两个字符串是同构的，如果s中的字符可以通过替换转换为t。
　　所有字符的出现都必须用另一个字符替换，同时保留字符的顺序。没有两个字符可以映射到同一个字符，但一个字符可以映射其本身。

## 思路：

　　核心就是建立映射表，把两个字符串中的字符相映射，判断两个字符串不是同构的根据是s中同一个字符映射t中两个不同的字符，或者t中同一个字符映射s中两个不同的字符，这样就会违反题目中定义的规则。如果遍历完两个字符串所有字符，没有出现上述情况，则两个字符串是同构字符串。

```c++
class Solution {
public:
	bool isIsomorphic(string s, string t) {
		if (Isomorphic(s,t)&&Isomorphic(t,s))
		{
			return true;
		}
		return false;	
	}
	bool Isomorphic(string s, string t) {
		map<char, char> hashmap;
		for (int i = 0; i < s.size(); i++)
		{
			if (hashmap[s[i]])
			{
				if (hashmap[s[i]] != t[i])
				{
					return false;
				}
			}
			else
			{
				hashmap[s[i]] = t[i];
			}
		}
		return true;
	}
};
```



