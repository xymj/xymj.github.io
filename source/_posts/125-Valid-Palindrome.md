---
title: 125. Valid Palindrome
date: 2017-08-21 22:41:15
tags: LeetCode
categories: LeetCode
---

# 125. Valid Palindrome

Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

For example,
"A man, a plan, a canal: Panama" is a palindrome.
"race a car" is not a palindrome.

Note:
Have you consider that the string might be empty ? This is a good question to ask during an interview.
For the purpose of this problem, we define empty string as valid palindrome.
<!--more-->

## 题意：

　　给定一个字符串，确定它是否是回文，只考虑字母数字字符和忽略空格。

## 思路：

　　利用前后两个指针，回文串肯定是关于中间对称的，所以利用前后指针遍历比较，遍历的过程中忽略空格、忽略大小写，当前指针大于后指针的时候，说明遍历结束是回文串，如果遍历过程中有一个字符不等则肯定不是回文串。

```c++
class Solution {
public:
    bool isPalindrome(string s) {
        int i = 0, j = s.size() - 1;
    	while (i < j) {
    	
    		if (!isalnum(s[i]))
    		{
    			i++;
    			continue;
    		}
    		if (!isalnum(s[j])) {
    			j--;
    			continue;
    		}
    		if (tolower(s[i]) == tolower(s[j]))
    		{
    			i++;
    			j--;
    			if (i > j)
    		    	return true;
    		}
    		else {
    			return false;
    		}
    	}
    	return true;
    }
};
```

