---
title: 14. Longest Common Prefix
date: 2017-07-03 21:06:59
tags: LeetCode
categories: LeetCode
---

# 14. Longest Common Prefix

Write a function to find the longest common prefix string amongst an array of strings.

## 题意：

　　在一个字符串数组中查找最长相同前缀。

<!--more-->

## 思路：

　　先找到字符串数组中最短的字符串，然后根据这个长度遍历每个string字符串中的字符，只要有一个字符串的字符不相等，则当前找的字符串就是最长相同前缀。

```c++
class Solution {
public:
	string longestCommonPrefix(vector<string>& strs) {
		string res = "";
		if (strs.empty())
			return res;
		int len = strs.size();
		int minStrLen = INT_MAX;
		for (int i = 0; i < len; i++) {
			minStrLen = min(minStrLen, (int)strs[i].size());
		}
		for (int j = 0; j < minStrLen; j++) {
			char val = strs[0][j];
			for (int i = 1; i < len; i++) {
				if (val == strs[i][j])
					continue;
				else
					return res;
			}
			res += val;
		}
		return res;
	}
};
```


```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (null == strs) {
            return "";
        }

        int len = strs.length;
        if (len == 0) {
            return "";
        }

        int minStrLen = Integer.MAX_VALUE;
        for (int i = 0;i < len;i++) {
            minStrLen = Math.min(minStrLen, strs[i].length());
        }

        String res = "";
        for (int j = 0;j < minStrLen;j++) {
            char cur = strs[0].charAt(j);
            for (int i = 1;i < len;i++) {
                char next = strs[i].charAt(j);
                if (cur == next) {
                    continue;
                } else {
                    return res;
                }
            }
            res += cur;
        }

        return res;

    }
}
```
