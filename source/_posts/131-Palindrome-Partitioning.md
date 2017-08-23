---
title: 131. Palindrome Partitioning
date: 2017-08-22 22:28:58
tags: LeetCode
categories: LeetCode
---

# 131. Palindrome Partitioning

Given a string s, partition s such that every substring of the partition is a palindrome.
Return all possible palindrome partitioning of s.

For example, given s = "aab",
Return
[
　　["aa","b"],
　　["a","a","b"]
]

<!--more-->

## 题意：

　　给定的字符串S，S的一个搜索子分区的分区是一个回文子串。返回所有可能的回文子串组合。

## 思路：

　　回溯递归法，因为要穷举出所有的所有的回文子串的组合，所以每层递归把当前层的回文子串放入结果集中，从下一个字符开始进入下一层，递归查找剩余子串中的回文子串组合，当递归遍历到字符串末尾的时候，说明一个回文子串组合查找完毕，放入结果集中，进行回溯递归上一层。**注意**：要当前层子串满足是回文串的时候才能递归进行下一层剩下字符串回文子串的查找。

```c++
class Solution {
public:
	vector<vector<string>> partition(string s) {
		vector<vector<string>> res;
		vector<string> tmp;
		getPartition(s, 0, tmp, res);
		return res;
	}
private:
	void getPartition(string& s, int idx, vector<string>& tmp, vector<vector<string>>& res) {
		if (idx == s.length()) {
			res.push_back(tmp);
			return;
		}
		for (int i = idx, n = s.length(); i < n; i++) {
			int l = idx, r = i;
			while (l < r && s[l] == s[r]) l++, r--;
			if (l >= r) {//控制满足回文子串才能进入下一层，减少递归层次，降低时间复杂度
				tmp.push_back(s.substr(idx, i - idx + 1));
				getPartition(s, i + 1, tmp, res);
				tmp.pop_back();
			}
		}
	}
};
```

