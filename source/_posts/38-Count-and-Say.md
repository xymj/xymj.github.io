---
title: 38. Count and Say
date: 2017-07-19 22:22:05
tags: LeetCode
categories: LeetCode
---

# 38. Count and Say

The count-and-say sequence is the sequence of integers beginning as follows:
1, 11, 21, 1211, 111221, ...

1 is read off as "one 1" or 11.
11 is read off as "two 1s" or 21.
21 is read off as "one 2, then one 1" or 1211.

Given an integer n, generate the nth sequence.
Note: The sequence of integers will be represented as a string.

<!--more-->

## 题意：

　　计数和说序列是整数序列，如下所示： 1, 11, 21, 1211, 111221, ...
　　也就是说：
　　　　　　n = 1<------------------>1
　　　　　　n = 2<------------------>11
　　　　　　n = 3<------------------>21
　　　　　　n = 4<------------------>1211
　　　　　　n = 5<------------------>111221　　
　　　　　　            ...............
　　每个n所对应的序列值，是通过n-1对应序列相同元素个数和此元素组合构成的。

## 思路：

　　如题意所述，n=1时输出字符串1；n=2时，数上次字符串中的数值个数，因为上次字符串有1个1，所以输出11；n=3时，由于上次字符是11，有2个1，所以输出21；n=4时，由于上次字符串是21，有1个2和1个1，所以输出1211。依次类推，可以直接递归实现。

```c++
class Solution {
public:
	string countAndSay(int n) {
		if (n==1)
		{
			return "1";
		}
		string s = countAndSay(n - 1);
		string res = "";
		int size = s.size();
		char flag = s[0];
		int count = 0,pos = 0;
		while (pos<size)
		{
			while(pos<size&&s[pos]==flag)
			{
				count++;
				pos++;
			}
			res += (to_string(count) + flag);
			count = 0;
			flag = s[pos];
		}
		return res;
	}
};
```

　　非递归实现：

```c++
class Solution {
public:
	string countAndSay(int n) {
		string curr_str = "1"; //n为1的时候，字符串为1
		for (int i = 0; i < n - 1; i++) { //对n-2序列的计数，就可以得到n-1的序列值（从零开始，n-1序列即使n的序列值）
			string buffer;
			for (int j = 0; j < curr_str.size(); j++) {
				int cnt = 0;
				char ch = curr_str[j];
				while (j < curr_str.size() && curr_str[j] == ch) {
					j++;
					cnt++;
				}
				buffer += (to_string(cnt) + ch);
				j--;
			}
			curr_str = buffer; // Update curr_str
		}
		return curr_str;
	}
};
```

---------------------------------------------------
#### 　Java Code：
```Java
class Solution {
    public String countAndSay(int n) {
        if (n == 1) {
            return String.valueOf(1);
        }
        String preStr = countAndSay(n - 1);
        int len = preStr.length();
        int count = 0;
        int i = 0;
        char flag = preStr.charAt(0);
        StringBuilder builder = new StringBuilder();
        while (i < len) {
            while (i < len && preStr.charAt(i) == flag) {
                count++;
                i++;
            }
            builder.append(count).append(flag);
            if (i >= len) {
                break;
            }
            flag = preStr.charAt(i);
            count = 0;
        }

        return builder.toString();
    }
}
```
