---
title: 20. Valid Parentheses
date: 2017-07-05 22:01:09
tags: LeetCode
categories: LeetCode
---

# 20. Valid Parentheses

Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.
The brackets must close in the correct order, "()" and "()[]{}" are all valid but "(]" and "([)]" are not.

<!--more-->

## 题意：

　　给定一个包含 '(', ')', '{', '}', '[' 和 ']'的字符串，判断括号对是否是有效的，即每个括号是否是成对出现的。

## 思路：

　　此题的做法主要是利用栈的特性，当是 '(', {', '[' 的时候直接入栈，当是')','}', ']' 的时候判断当前栈顶元素是否和又括号匹配，如果正常匹配则弹出，不匹配遍历下一个下一个字符，最后判断栈大小是否为空，为空则说明匹配正确有效。

```c++
class Solution {
public:
	bool isValid(string s) {
		if (s.empty())
			return false;
		stack<char> st;
		for (auto res : s)
		{
			if (res == ')'&&!st.empty()&&st.top() == '(')
				st.pop();
			else if (res == '}'&&!st.empty()&&st.top() == '{')
				st.pop();
			else if (res == ']'&&!st.empty()&&st.top() == '[')
				st.pop();
			else
				st.push(res);
		}
		if (st.empty())
			return true;
		else
			return false;
	}
};
```

