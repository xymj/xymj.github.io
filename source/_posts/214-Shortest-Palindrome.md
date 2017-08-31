---
title: 214. Shortest Palindrome
date: 2017-08-29 22:45:54
tags: LeetCode
categories: LeetCode
---

Given a string S, you are allowed to convert it to a palindrome by adding characters in front of it. Find and return the shortest palindrome you can find by performing this transformation.

For example:
Given "aacecaaa", return "aaacecaaa".
Given "abcd", return "dcbabcd".

<!--more-->

## 题意：

　　 给定一个字符串，在字符串的前面添加字符将它转换成一个回文串。找到并返回可以通过执行这个变化后转换的最短回文串。 

## 思路：

### 　方法一：

　　在s前面添加字符串出现最短的回文串，就是找到距离第一个元素最近的对称点，左侧形成对称的话，如果右侧还有元素存在，只需反转加到s的前面即可。   其中更加注意的一点就是：对称的对称轴元素可能为一个，也可能为两个。
　　从某个char开始向两边扩展(左右两边的字符相等), 如果能一直扩展到字符串的头部, 则将末尾余下的reverse,再加到原字符串的头部。

1. 中轴字符选从中间开始,这样找到的即为最短的. 
2. 中轴字符可能为一个, 也可能为两个.

```c++
//此法效率不是很高
class Solution {
public:
	string shortestPalindrome(string s) {
		int sLen = s.size();
		if (sLen <= 1)
			return s;
		string res = "";
		int center = sLen / 2;
		for (int i = center; i >= 0; i--)//从中间往左边查找对称点
		{
			if (s[i]==s[i+1])
			{
				res = isPalindrome(s, i, i+1);//偶数的情况
				if (res!="#")
				{
					return res;
				}
			}
			//else//切记此处不是else，也就是说，可能即使s[i]==s[i+1]的情况也会出现奇数个的情况,例如：aaaaa
			//{
			res = isPalindrome(s, i, i);//奇数的情况
			if (res != "#")
			{
				return res;
			}
			//}
		}
		return res;
	}

	string isPalindrome(string s, int l, int r)
	{
		int i = 1;//i表示从中间元素向两边扩展的距离
		for (; l - i >= 0 && r + i < s.length(); i++) {
			if (s[l - i] != s[r + i]) break;
		}
		if (l - i >= 0)//说明不是最靠近左侧的对称点，返回#标识 	
          return "#";
		string tempStr = s.substr(r + i);
		reverse(tempStr.begin(), tempStr.end());
		s = tempStr + s;
		return s;
	}
};
```

### 　方法二：

　　下面的算法主要依据KMP算法，实现主串和模式串的匹配，其中主串是s的反转串，模式串是s。用kmp字符串匹配算法就可以完美地完成O(n)复杂度的算法。只需对kmp做一点点调整：
　　　判断匹配结束的时机：是reverse_s串匹配到了尾部，且匹配成功。
　　　用mark记录匹配到结尾时s串的下标位置：为了从s串尾部截取子串接到reverse_s后形成回文字符串。
　　　如何保证最短：由于第一次成功匹配到reverse_s尾部后就结束循环，此时的mark标记的位置所形成的回文段（字符串s从0到mark下标所形成的子串）应该是最长的。
　　使用c++自带stl函数：reverse操作，substr操作。

```c++
class Solution {
public:
	string shortestPalindrome(string s) {
		int sLen = s.size();
		if (sLen <= 1)
			return s;
		string res = "";
		string reverse_s(s);
		reverse(reverse_s.begin(), reverse_s.end());
		vector<int> next(sLen, 0);
		getNext(s, next);
		int i = 0, j = 0,mark = 0;
		while (i<sLen&&j<sLen)
		{
			if (j == -1|| reverse_s[i] == s[j])
			{
				i++;
				j++;
				if (i == sLen)
				{
					mark = j;
					break;
				}
			}
			else
			{
				
				j = next[j];
			}
		}
		res = mark == sLen ? s : reverse_s + s.substr(mark);
		return res;
	}
	//求模式串的next数组的方法
	void getNext(string s, vector<int> &next) {
		int sLen = s.size();
		int k = -1;
		int j = 0;
		next[0] = -1;
		while (j<sLen-1)
		{
			if (k == -1 || s[k] == s[j])
			{
				k++;
				j++;
				if (s[k] == s[j])
				{
					next[j] = next[k];
				}
				else
					next[j] = k;
			}
			else
				k = next[k];
		}
	}
};
```

### 　方法三：

　　思路：让在前面补一些字符使得给定的字符串变成回文，观察可以发现我们需要添加多少个字符与给定字符串的前缀子串回文的长度有关．也就是说去掉其前缀的回文子串，我们只需要补充剩下的子串的逆序就行了。

　　举个例子：
　　　aacecaaa，其前缀的最大回文子串是aacecaa，剩下了一个a，因此我们只需要在前面加上一个a的逆序a就行了．再例如abcd，其前缀的最大回文是a，因此剩下的子串就为bcd，所以需要在前面加上bcd的逆序，就变成了dcbabcd．所以这样问题就转化为求字符串的前缀最大回文长度．

　　一个naive的方法是先判断整个字符串是否回文，否的话再判断前n-1个子串是否回文，这样依次缩减长度，直到找到一个回文子串就是最大的前缀回文子串．这种方法简单粗暴，容易理解和实现，如果在面试中要求不是很严格的情况下说不定可以过关, 反正总比不会好点^.^．其时间复杂度是O(n!)．

　　KMP，这是一个非常高效的字符串比较算法．其原理是给定一个字符串S和P，要在S中寻找是否存在P，一般的方法是逐位比较，如果不能完全匹配，则S再回到开始位置向右移动一位，P回到0位置再开始比较．在KMP中不需要回到首部重新开始比较，借助与记录从P的开头到当前位置P中的前缀和后缀有多少位是相等的，这样当P和S比较的时候如果P[i] != S[j]了，不需要回到P[0]的位置重新比较，我们可以查看P中已经匹配过的子串中（也就是P[0, i-1]子串）前缀和后缀有多少位是相等的，然后将P的前缀和已经S[j]之前的后缀是匹配的，就可以不用回溯S了．所以借助与KMP记录最长前缀和后缀的方法，我们可以将原字符串翻转以后加在原字符串的后面，其最大的前缀和后缀就是前缀的最大回文长度．我们还需要在这两个字符串之间加一个冗余字符，因为形如aaaaa这种字符串如果不加一个冗余字符最大前缀和后缀会变大．

```c++
//效率高，但是不太理解
class Solution {
public:
	string shortestPalindrome(string s) {
		int sLen = s.size();
		if (sLen <= 1)
			return s;
		string res = "";
		string reverse_s(s);
		reverse(reverse_s.begin(), reverse_s.end());
		string str = s + "#" + reverse_s;
		int strLen = str.size();
		vector<int> maxPalindromeVec(strLen, 0);
		for (int i =1;i<strLen;i++)
		{
			int k = maxPalindromeVec[i - 1];
			while (k>0&&str[k]!=str[i])
			{
				k = maxPalindromeVec[k - 1];
			}
			maxPalindromeVec[i] = (k += str[k] == str[i]);
		}
		return str.substr(sLen + 1, sLen - maxPalindromeVec[strLen - 1]) + s;
	}
};
```

