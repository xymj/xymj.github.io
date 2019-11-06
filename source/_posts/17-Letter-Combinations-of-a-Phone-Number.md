---
title: 17. Letter Combinations of a Phone Number
date: 2017-07-04 21:29:59
tags: LeetCode
categories: LeetCode
---

# 17. Letter Combinations of a Phone Number

Given a digit string, return all possible letter combinations that the number could represent.
A mapping of digit to letters (just like on the telephone buttons) is given below.

Input:Digit string "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].

Note:
Although the above answer is in lexicographical order, your answer could be in any order you want.

<!--more-->

## 题意：

　　给定一个数字字符串，每个数字对应手机键盘上的英文字母组合，返回所有数字可以代表的字符组合。

## 思路：

　　经典的backtracking（回溯算法）的题目。当一个题目，存在各种满足条件的组合，并且需要把它们全部列出来时，就可以考虑backtracking了。但是，backtracking在一定程度上属于穷举，所以当数据特别大的时候，不合适。而对于那些题目，可能就需要通过动态规划来完成。
　　这道题的思路很简单，假设输入的是"23"，2对应的是"abc"，3对应的是"edf"，那么我们在递归时，先确定2对应的其中一个字母（假设是a），然后进入下一层，穷举3对应的所有字母，并组合起来（"ae"，"ad"，"af"），当"edf"穷举完后，返回上一层，更新字母b，再重新进入下一层。这个就是backtracing的基本思想。
　　**特别注意**0和1在手机键盘上不对应字符。

```c++
class Solution {
public:
	vector<string> letterCombinations(string digits) {
		vector<string> res;
		if (digits.empty())
		{
			return res;
		}
		vector<string> hashMap = { "","","abc" ,"def" ,"ghi" ,"jkl" ,"mno" ,"pqrs" ,"tuv" ,"wxyz" };//0,1下标对应的是空字符串
		letterCombina(digits, hashMap, res, 0, "");
		return res;
	}
	void letterCombina(string digits, vector<string> hashMap, vector<string> &res, int index, string cutStr)//返回上一层要记录结果值，左移用引用参数
	{
		if (index==digits.size())
		{
			if (!cutStr.empty())
			{
				res.push_back(cutStr);
				return;
			}
		}
		string tempStr= hashMap[atoi(digits.substr(index,1).c_str())];
		for (int i = 0;i<tempStr.size();i++)
		{
			string nextCutStr = cutStr+tempStr[i];
			letterCombina(digits, hashMap, res, index + 1, nextCutStr);
		}
	}
};

```

---------------------------------------------------
#### 　Java Code：
```java
class Solution {
    private String[] letters = {"abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    private List<String> ans = new ArrayList<>();
    public List<String> letterCombinations(String digits) {
        if (digits.length() == 0) return ans;
        track(digits, 0, "");
        return ans;
    }

    private void track(String digits, int index, String str) {
        if (index == digits.length()) {
            ans.add(str);
            return;
        }

        int len = letters[digits.charAt(index) - '2'].length();
        for (int j = 0; j < len; j++) {
            track(digits, index + 1, str + letters[digits.charAt(index) - '2'].charAt(j));
        }
    }
}
```

```java
class Solution {
    public List<String> letterCombinations(String digits) {
        List<String> res = new ArrayList();
        if (null == digits) {
            return res;
        }

        int len = digits.length();
        if (len == 0) {
            return res;
        }
        Map<Character,String> phoneNumHash = new HashMap();
        phoneNumHash.put('0', "");
        phoneNumHash.put('1', "");
        phoneNumHash.put('2', "abc");
        phoneNumHash.put('3', "def");
        phoneNumHash.put('4', "ghi");
        phoneNumHash.put('5', "jkl");
        phoneNumHash.put('6', "mno");
        phoneNumHash.put('7', "pqrs");
        phoneNumHash.put('8', "tuv");
        phoneNumHash.put('9', "wxyz");

        letterCombinations(digits, 0, res, "", phoneNumHash);
        return res;
    }

    public void letterCombinations(String digits,int curIndex,List<String> res,String curStr,Map<Character,String> numHash){
        if (curIndex == digits.length()) {
            if (null != curStr && curStr.length() != 0) {
                res.add(curStr);
                return;
            }
        }

        String numStr = numHash.get(digits.charAt(curIndex));
        if (null == numStr) {
            return;
        }

        for (int i = 0;i < numStr.length();i++) {
            letterCombinations(digits, curIndex + 1, res, curStr + numStr.charAt(i), numHash);
        }
    }
}
```
