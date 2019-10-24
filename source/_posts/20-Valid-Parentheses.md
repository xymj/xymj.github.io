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

　　给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

　　有效字符串需满足：
    　　左括号必须用相同类型的右括号闭合。
    　　左括号必须以正确的顺序闭合。

注意空字符串可被认为是有效字符串。

## 思路：

　　此题的做法主要是利用栈的特性，当是 '(', {', '[' 的时候直接入栈，当是')','}', ']' 的时候判断当前栈顶元素是否和右括号匹配，如果正常匹配则弹出，不匹配遍历下一个下一个字符，最后判断栈大小是否为空，为空则说明匹配正确有效。

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

```java
class Solution {
    public boolean isValid(String s) {
        if (null == s) {
            return false;
        }
        int len = s.length();
        if (len == 0) {
            return true;
        }
        Stack<Character> stack = new Stack();
        for (int i = 0;i < len;i++) {
            char cur = s.charAt(i);
            if (cur == ')' && !stack.empty()
               && '(' == stack.peek()) {
                stack.pop();                
            } else if (cur == '}' && !stack.empty()
                      && '{' == stack.peek()) {
                stack.pop();               
            } else if (cur == ']' && !stack.empty()
                      && '[' == stack.peek()) {
                stack.pop();               
            } else {
                stack.push(cur);
            }
        }
        return stack.empty();
    }
}
```
