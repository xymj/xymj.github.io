---
title: 58. Length of Last Word
date: 2017-07-26 22:20:05
tags: LeetCode
categories: LeetCode
---

# 58. Length of Last Word

Given a string s consists of upper/lower-case alphabets and empty space characters ' ', return the length of last word in the string.

If the last word does not exist, return 0.

Note: A word is defined as a character sequence consists of non-space characters only.

For example,
Given s = "Hello World",
return 5.

<!--more-->

## 题意：

　　给定一个字符串s由大/小写字母和空格字符组成，返回字符串中最后一个单词的长度。如果最后一个单词不存在，返回0。

## 思路：

　　考虑特殊情况三种情况：
　　　　1.整个字符串为空时
　　　　2.字符串由无数的空格组成时
　　　　3.字符串最后以空格结尾时

```c++
class Solution {
public:
	int lengthOfLastWord(string s) {
		int res = 0;
		if (s.empty() || s == " ")
			return 0;
		auto itr = s.find_last_of(' ');
		while (itr == s.size() - 1) {
			if (itr == 0)
			{
				return res;
			}
			s = s.substr(0, itr);
			itr = s.find_last_of(' ');
		}
		s = s.substr(itr + 1);
		return s.size();
	}
};
```

---------------------------------------------------
### 　Java Code
```Java
class Solution {
    public int lengthOfLastWord(String s) {
        if (null == s) {
            return 0;
        }  

        int size = s.length();
        if (size == 0) {
            return 0;
        }

        int end = size - 1;
        while (end >= 0 && s.charAt(end) == ' ') {
            end--;
        }
        if (end < 0) {
            return 0;
        }

        int start = end;
        while (start >= 0 && s.charAt(start) != ' ') {
            start--;
        }

        return end - start;
    }
}
```
