---
title: 5. Longest Palindromic Substring
date: 2017-06-28 17:40:22
tags: LeetCode
categories: LeetCode
---

# 5. Longest Palindromic Substring

Given a string S, find the longest palindromic substring in S. You may assume that the maximum length of S is 1000, and there exists one unique longest palindromic substring.



<!-- more -->

## 题意：

　　给一个字符串，找到这个字符串的最长回文子串。可以假设给定的字符串的长度最长为1000个字符，而且一定存在一个唯一的最长回文子串。

## 思路：

　　判断一个字符串是否是回文串，主要有两种方式判断：

    　　　　1. 两个指针从两端到中间，如果左右指针不出现不相等的字符，则此字符串是回文串。

    　　　　2. 两个指针从中间到两端，即中心扩展法，如果左右指针不出现不相等的字符，则此字符串是回文串，注意此方法回文字符串字符数分为奇、偶两种类型。

　　此题主要采用方法二，从前往后遍历字符串，每个字符都有可能是回文字符串的中点。依次求得各字符作为中心的回文字符串，取其中最长的那个即是结果。

```c++
class Solution {
public:
	string longestPalindrome(string s) {
		string res = "";
		if (s.size()<=1)
			return s;
		for (int i =0;i<s.size();i++)
		{
			string tempPalindrome1 = isPalindrome(s, i, i);//奇数的情况
			if (tempPalindrome1.size()>res.size())
			{
				res = tempPalindrome1;
			}
			string tempPalindrome2 = isPalindrome(s, i, i+1);//偶数的情况
			if (tempPalindrome2.size() > res.size())
			{
				res = tempPalindrome2;
			}
		}
		return res;
	}
	string isPalindrome(string s,int pre,int tail)
	{
		while (pre>=0&&tail<s.size()) {
			if (s[pre] == s[tail]) {
				pre--;
				tail++;
				continue;
			}
			else
				break;
		}
		return s.substr(pre+1,tail-pre-1);
	}
};
```

```java
class Solution {
    public String longestPalindrome(String s) {
        if (null == s) {
            return "";
        }

        int len = s.length();
        String res = "";
        for (int i = 0;i < len;i++) {
            String palindrome1 = getPalindromeStr(s, i, i);
            if (palindrome1.length() > res.length()) {
                res = palindrome1;
            }

            String palindrome2 = getPalindromeStr(s, i, i + 1);
            if (palindrome2.length() > res.length()) {
                res = palindrome2;
            }
        }

        return res;
    }

    public String getPalindromeStr(String s,int left, int right) {
        while(left >=0 && right < s.length()) {
            char leftChar = s.charAt(left);
            char rightChar = s.charAt(right);
            if (leftChar == rightChar) {
                left--;
                right++;
            } else {
                break;
            }
        }

        return s.substring(left + 1, right);
    }
}
```
