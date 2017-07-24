---
title: 49. Group Anagrams
date: 2017-07-22 14:11:15
tags: LeetCode
categories: LeetCode
---

# 49. Group Anagrams

Given an array of strings, group anagrams together.

For example, given: ["eat", "tea", "tan", "ate", "nat", "bat"],
Return :
　　[
　　　　["ate", "eat", "tea"],
　　　　["nat", "tan"],
　　　　["bat"]
​	]
Note: All inputs will be in lower - case.

<!--more-->

## 题意：

　　给定一个字符串数组，把字符相同的字符串放在一起。所有的输入都是小写字母。

## 思路：

　　主要是利用hash映射表来判断含有相同字符的字符串，但是字符串中的字符顺序不同，所以要对每个string字符转进行字典排序，由于对单词排序时，题目已经限定了单词只可能是26个小写字母组成的，所以可以使用计数排序，进一步加快算法的速度（排序部分速度从O(nlogn)变为O(n)）。

```c++
class Solution {
public:
	vector<vector<string>> groupAnagrams(vector<string>& strs) {
		vector<vector<string>> res;
		if (strs.empty())
		{
			return res;
		}
		map<string, vector<string>> hash;
		sort(strs.begin(), strs.end());
		for (int i = 0;i<strs.size();i++)
		{
			string t = strs[i];
			t = sortStr(t);//像后面Internet上一样，利用计数排序提高效率
			if (hash.count(t))
			{
				hash[t].push_back(strs[i]);
			}
			else
			{
				vector<string> tempS;
				tempS.push_back(strs[i]);
				hash.insert(make_pair(t, tempS));
			}
		}
		for (auto tr : hash)
		{
			res.push_back(tr.second);
		}
		return res;
	}
	string sortStr(string s)//计数排序，提高排序效率
	{
		vector<int> counts(26,0);
		for (auto c : s)
		{
			++counts[c - 'a'];
		}
		string res="";
		for (int i = 0;i<26;i++)
		{
			while (counts[i])
			{
				res += (i + 'a');
				counts[i]--;
			}
		}
		return res;
	}
};

```

