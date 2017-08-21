---
title: 115. Distinct Subsequences
date: 2017-08-16 23:07:50
tags: LeetCode
categories: LeetCode
---

# 115. Distinct Subsequences

Given a string S and a string T, count the number of distinct subsequences of T in S.

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, "ACE" is a subsequence of "ABCDE" while "AEC" is not).

Here is an example:
S = "rabbbit", T = "rabbit"

Return 3. 

<!--more-->

## 题意：

　　给定一个字符串S和字符串T，统计不同的子序列T的在S中出现的数量。
　　字符串的子序列是一个新字符串，它从原始字符串中形成，通过删除一些（可以是没有）字符而**不干扰其余字符的相对位置**。（例如“ACE”是“ABCDE”的一个子序列，而“AEC”不是）。

## 思路：

　　题目给定两个字符串，选择只可以用删除字符的方法从第一个字符串变换到第二个字符串，求出一共有多少种变换方法；动态规划思想应用的典型题目。

```c++
定义二维数组dp[i][j]为s[0, i)中包含多少个值为t[0, j)的子序列。

先考虑几个基本情况：
	（1）对于dp[0][0]：
        此时s[0, 0)和t[0, 0)都是空串，我们可以说前者是包含一个值为空串的子序列的，因此dp[0][0] = 1。
    （2）对于dp[0][j] (j >= 1)：
        s[0, 0)是空串，而t[0, j)不是空串，因此前者不可能含有值为后者的子序列。此时，dp[0][j] = 0。
    （3）对于dp[i][0] (i >= 1)：
        s[0, i)不是空串，而t[0, 0)是空串，我们认为前者包含一个值为空串的子序列。因此，dp[i][0] = 1。
    （4）对于dp[i][j] (i >= 1 and j >= 1)：
            如果s[i - 1] == t[j - 1]，那么：dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j]。意思是：如果当前S[i]==T[j]，那么当前这个字母即可以保留也可以抛弃，所以变换方法等于保留这个字母的变换方法加上不用这个字母的变换方法。
            如果二者不等，那么：dp[i][j] = dp[i - 1][j]。意思是：如果当前字符不等，那么就只能抛弃当前这个字符。
```

```c++
class Solution {
public:
	int numDistinct(string s, string t) {
		if (s.empty())
		{
			return 0;
		}
		int sLen = s.size();
		int tLen = t.size();
		if (sLen<tLen)
		{
			return 0;
		}
		vector<vector<int>>   dp(sLen + 1, vector<int>(tLen + 1, 0));
		for (int i=0;i<sLen+1;i++)
		{
			dp[i][0] = 1;
		}
		for (int i =1;i<sLen+1;i++)
		{
			for (int j = 1;j<tLen+1;j++)
			{
				if (s[i - 1] == t[j - 1])
				{
					dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
				}
				else
					dp[i][j] = dp[i - 1][j - 1];
			}
		}
		return dp[sLen][tLen];
	}
};

```

