---
title: 22. Generate Parentheses
date: 2017-07-07 22:54:58
tags: LeetCode
categories: LeetCode
---

# 22. Generate Parentheses

Given n pairs of parentheses, write a function to generate all combinations of well - formed parentheses.

For example, given n = 3, a solution set is :
"((()))", "(()())", "(())()", "()(())", "()()()"

<!--more-->

## 题意：

　　给定n对括号，编写一个函数来生成圆括号的所有组合。

## 思路：

　　首先确定递归终止的条件，当n == 0的时候，说明没有括号对，直接返回空字符串，当n == 1的时候，说明只有一个括号对，那么组合的情况也就有一种()，直接返回即可，单n >= 2的时候进行递归运算，每次递归下一层去计算n - 1个括号对所产生的括号组合情况，当递归返回的时候，遍历返回的所有组合对，为每一种的组合情况都加入一个括号对“()”,这个括号对的位置是n - 1个括号对字符串的任何一个位置，这样就能把n个括号对的所有情况都遍历罗列出来。
　　**特别注意**重复的括号对字符串，要用set容器进行过滤。

```c++
//利用递归进行求解，要以后用树的思想求解
vector<string> generateParenthesis(int n) {
	vector<string> res;
	if (n == 0)
		res.push_back("");
	if (n == 1) {
		res.push_back("()");
	}
	else {
		vector<string> pre_vec = generateParenthesis(n - 1);
		set<string> filter_set;//过滤重复的字符串
		int sizes = pre_vec.size();
		for (int i = 0; i < sizes; ++i) {
			string temp_str = pre_vec[i];
			for (int j = 0; j < temp_str.size(); j++) {
				string now_str = temp_str.substr(0, j) + "()" + temp_str.substr(j, temp_str.size());
				filter_set.insert(now_str);
			}
		}
		for (auto &r : filter_set) {
			res.push_back(r);
		}
	}
	return res;
}
```

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> res = new ArrayList();
        if (n == 0) {
            return res;
        }
        if (n == 1) {
            res.add("()");
            return res;
        }

        List<String> preParenthesis = generateParenthesis(n - 1);
        int size = preParenthesis.size();
        for (int i = 0;i < size;i++) {
            String tmp = preParenthesis.get(i);
            for (int j = 0;j < tmp.length();j++) {
                String tmpRes = new StringBuilder(tmp.substring(0, j))
                    .append("()").append(tmp.substring(j, tmp.length())).toString();
                res.add(tmpRes);
            }

        }

        return res.stream().distinct().collect(Collectors.toList());
    }
}
```
