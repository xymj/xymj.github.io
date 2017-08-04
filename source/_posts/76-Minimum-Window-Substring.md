---
title: 76. Minimum Window Substring
date: 2017-08-04 21:55:14
tags: LeetCode
categories: LeetCode
---

# 76. Minimum Window Substring 

Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).

For example,
S = "ADOBECODEBANC"
T = "ABC"
Minimum window is "BANC".

Note:
If there is no such window in S that covers all characters in T, return the empty string "".
If there are multiple such windows, you are guaranteed that there will always be only one unique minimum window in S.
<!--more-->

## 题意：

　　给定一个字符串S和一个字符串T，找到S中的最小窗口，该窗口包含T中的所有字符。

## 思路：

### 　方法一：

　　这道题的要求是在给定的字符串S中找到最小的窗口使其完全包含字符串T中所有字符，如果不存在，则返回空串""。和Longest Substring Without Repeating Characters及Substring with Concatenation of All Words类似的思路，使用l和r两个指针维护子串，用Hash表记录T字串中出现的字符。S中，每次循环r右移1位，然后判断r右移之后所指向的字符是否在Hash表中出现过：如果出现过，则表示在T中。此时通过计数器cnt判断T中字符是否  ""都""  出现过，如果是，则记录l和r之间子串长度，并与最短长度比较。然后逐步右移l并在Hash表中删除l指向的字符直到计数器cnt小于T中字符数量。
　　**注意**由于T中同一字符的数量可能减到负值，因此需要2重判断：先判断是否出现此字符，在判断此字符出现的具体数量。
　　时间复杂度：O(l1+l2)（l1和l2分别为字符串S和T的长度）
　　空间复杂度：O(l2)

```c++
class Solution {
public:
	string minWindow(string s, string t) {
		string res = "";
		unordered_map<char, int> hash;
		for (int i = 0; i<t.size(); i++)
		{
			hash[t[i]]++;
		}
		int countT = 0, left = 0, right = 0, minLeft = 0, minSize = INT_MAX;
		//hash里面键所对应的值是在left到right之间所有出现过的是t中元素的值，每出现一个对应的值减一，可能出现负值是因为left到right之间包好的t中元素个数多余t中要匹配的个数，所以当left向右移动（countT--的时候），只有值刚好等于0的时候count记录的才是t中元素个数。
		//关键点就是countT是记录的窗体中有几个目标字符，用其来控制缩小窗体，而hash中值得总数是窗体中所有目标值得个数。
		for (;right<s.size();right++)
		{
			if (hash.count(s[right]))
			{
				hash[s[right]]--;
				if (hash[s[right]]>=0)
				{
					countT++;
				}

				while (countT == t.size())
				{

					if (right - left + 1 < minSize)
					{
						minLeft = left;
						minSize = right - left + 1;
					}
					if (hash.count(s[left]))
					{
						hash[s[left]]++;
						if (hash[s[left]] > 0)
						{
							countT--;	
						}
					}
					left++;
				}
			}
		}
		if (minSize>s.size())
		{
			return res;
		}
		res = s.substr(minLeft, minSize);
		return res;
	}
};
```

### 　方法二：

　　思路类似方法一，但是没有利用map容器，使用数组来完成相应字符与其数量的映射，由于大小写字母的ASCII码不大于128，因此开辟两个数组存储信息。needFind数组存储T字符串每个字符出现次数。例如：needFind['A']=5意为T中A出现5次。Found数组存储S字符串在[begin,end]窗口内每个字符出现次数。
　　算法核心思想如下：
　　　　在保证[begin,end]窗口包含T中所有字符的条件下，延伸end，收缩begin。
　　　　进行一次扫描后，记录符合条件的最小窗口(end-begin+1)表示的字符串。
　　　　使用count记录剩余“有效”字符数，当count达到0时，即可说明[begin,end]包含了T。
　　　　注意：“有效”的意思是指，当前延伸得到的S[end]字符，使得[begin,end]更进一步包含T，而不是重复劳动。
　　　　比如说，T="a", [begin,end]已经包含"a",再延伸得到"aa"，只是无效操作，并没有使得[begin,end]更接近T,有效字符数仍为1.

```c++
class Solution {
public:
    string minWindow(string S, string T) {
        int begin = 0;
        int end = 0;
        int minbegin = 0;
        int minend = 0;
        int minSize = INT_MAX;
        vector<int> needFind(128, 0);
        vector<int> Found(128, 0);
        for(int i = 0; i < T.size(); i ++)
            needFind[T[i]] ++;
        Found[S[0]] ++;
        int count = T.size();
        if(needFind[S[0]] >= Found[S[0]])
            count --;
        while(true)
        {
            if(count == 0)
            {//shrink begin
                while(Found[S[begin]] > needFind[S[begin]])
                {
                    Found[S[begin]] --;
                    begin ++;
                }
                int size = end-begin+1;
                if(size < minSize)
                {
                    minbegin = begin;
                    minend = end;
                    minSize = size;
                }
            }
            if(end < S.size())
            {
                end ++;
                Found[S[end]] ++;
                if(needFind[S[end]] >= Found[S[end]])
                    count --;
            }
            else
                break;
        }
        if(minSize != INT_MAX)
            return S.substr(minbegin, minSize);
        else
            return "";
    }
};
```

