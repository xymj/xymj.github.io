---
title: 30. Substring with Concatenation of All Words
date: 2017-07-17 22:36:14
tags: LeetCode
categories: LeetCode
---

# 30. Substring with Concatenation of All Words

You are given a string, s, and a list of words, words, that are all of the same length.Find all starting indices of substring(s) in s that is a concatenation of each word in words exactly once and without any intervening characters.

For example, given:
s: "barfoothefoobarman"
words : ["foo", "bar"]
You should return the indices : [0, 9].
(order does not matter).

<!--more-->

## 题意：

　　给定一个字符串S，一个单词列表L（所有单词均等长），在S中找到包含L中所有单词连续出现在S中的首位置组成的集合（单词元素顺序不考虑），**注意**组成的要查找子串是单词连续组成，没有字符隔断，返回符合该要求的所有子串的首位置。

## 思路：

解决该问题的关键是理解清楚题目要求。
给定一个目标字符串s，一个单词集合words。
要求使得words集合中所有元素连续出现在s中的首位置组成的集合（元素顺序不考虑）。

正如所给实例，目标字符串s: “barfoothefoobarman”
对比单词集合words: [“foo”, “bar”]
我们发现，在pos=0 ~ 5时“barfoo”恰好匹配，则0压入结果vector；
在pos=9 ~ 14时“foobar”恰好匹配，则9压入结果vector；

```c++
class Solution {
	//s: "barfoothefoobarman"
	//	words : ["foo", "bar"]
public:
	vector<int> findSubstring(string s, vector<string>& words) {
		vector<int> res;
		unordered_map<string, int> hash;//记录所给words中每个单词的出现次数
		for (int i = 0; i < words.size(); i++)
		{
			hash[words[i]]++;
		}
		unordered_map<string, int> tempHash;
		int wordSize = words[0].size(); //每个单词的长度相同
		int wordsNum = words.size();
		int sLen = s.size();//所给匹配字符串的长度
		int left = 0, right = 0;
		for (; left < s.size() - wordSize*wordsNum + 1; left++)
		{
			tempHash.clear();
			for (right = 0;right< wordsNum;right++)
			{
				string tempS = s.substr(left + right*wordSize, wordSize);//检验当前单词是否属于words以及出现的次数是否一致
				if (hash.count(tempS))
				{
					tempHash[tempS]++;
					if (tempHash[tempS]>hash[tempS])//如果出现的次数与words不一致，则返回错误
					{
						break;
					}
				}
				else
					break;
			}
			if (right == wordsNum) //所有words内的单词,在left起始位置都出现，则将下标i存入结果的vector中
			{
				res.push_back(left);
			}
		}
		return res;
	}
};
```

