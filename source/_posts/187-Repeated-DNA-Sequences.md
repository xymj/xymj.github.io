---
title: 187. Repeated DNA Sequences
date: 2017-08-26 16:49:25
tags: LeetCode
categories: LeetCode
---

All DNA is composed of a series of nucleotides abbreviated as A, C, G, and T, for example: "ACGAATTCCG".When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.

Write a function to find all the 10 - letter - long sequences(substrings) that occur more than once in a DNA molecule.

For example
Given s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT",
Return:
["AAAAACCCCC", "CCCCCAAAAA"].

<!--more-->

## 题意：

　　所有的DNA都是由简写为A，C，G，T的核苷酸构成的，例如ACGAATTCCG。在研究DNA时，辨别DNA中重复的序列在有些时候是很有用的。
　　编写一个程序来找到一个DNA分子中出现次数超过一次的长度为10的子序列（子串）。

## 思路：

　　如果时间上没有要求的话，用find和substr搞定是没问题的，然而在测试数据较大的时候这种方法必然会超时。这个题的标签是Hash Table和Bit Manipulation，参考https://leetcode.com/discuss/24478/i-did-it-in-10-lines-of-c中的方案，需要利用hash表，以及按位运算来解决此题。
　　测试数据中只会出现4种字符’A’,’C’,’G’,’T’，其ASC值分别为65,67,71,84,对应的二进制位如下所示，这里仅列取32位情况下的低8位，前24位全部为0。
　　　　　　A --- 65 --- 0100 0001
　　　　　　C --- 67 --- 0100 0011
　　　　　　G --- 71 --- 0100 0111
　　　　　　T --- 84 --- 0101 0100
　　可以发现，仅通过低3位就能把4种字符区分开来，题目又要求了是长度为10的子串（10-letter-long），试想为什么不是11或者更大呢，因为10个字符每个用3位表示一个字符的话刚好是30位，而一个int是32位，刚好能放下，如果是11+就放不下了。因此，我们可以把一个长度为10的字符串映射成一个int数，用其低30位表示这个串，作为这个串的key，然后把key存在Hash Table中，当key重复出现时就代表子串重复出现了。

　　除了上述方法外，受其启发，其实只用2个二进制位就可以唯一区分4中字符，比如A-00,C-01,G-10,T-11，这样，我们只用int的低20位就可以表示一个字符串的key，这种方法甚至可以最多用于处理长度为16的子串。比上面一种方法略显复杂的是，需要手动将ACGT映射成2位二进制数，方法也是多种多样的。比如：

```c++
//转换2位二进制数方法一：
int function(char c) 
{
  int ret = 0;
  switch (c) {
      case 'A': break;
      case 'G': ret = 1; break;
      case 'C': ret = 2; break;
      case 'T': ret = 3; break;
  }
  return ret;
}

//转换2位二进制数方法二：
// 数组映射，调用时使用nums[c - 65]即可得到字符c的映射码
int nums[20]; 
nums[0] = 0; 
nums[2] = 1; 
nums[6] = 2; 
nums[19] = 3;

//转换2位二进制数方法三：
//(s[i] - 64) % 5完成映射
 A : ('A' - 64) % 5 = 1  (mod 5) = 1 = 01
 B : ('C' - 64) % 5 = 3  (mod 5) = 3 = 11
 C : ('G' - 64) % 5 = 7  (mod 5) = 2 = 10
 D : ('T' - 64) % 5 = 20 (mod 5) = 0 = 00
key = key << 2 & 0xfffff | (s[i] - 64) % 5;
```
### 方法一：

　　用ACGT的ASC码的后2位来映射的解法，然后做移位操作，相与操作后，下面代码中&0xfffff应该保证后20位相等，即加入一个字符构成的序列的key值，只要重复前面出现，经过key = ((key << 2) | ((s[i] - 64) % 5))&0xfffff;计算的key值就会相等。

```c++
class Solution {
public:
	vector<string> findRepeatedDnaSequences(string s) {
		vector<string> res;
		if (s.size()<10)
		{
			return res;
		}
		unordered_map<int, int> hash;
		int key = 0, i = 0;
		for (;i<9;++i)
		{
			key = ((key << 2) | ((s[i] - 64) % 5))&0xfffff;
		}
		for (;i<s.size();++i)
		{
			key = ((key << 2) | ((s[i] - 64) % 5))&0xfffff;
			int val = hash[key];
			hash[key]++;
			if (val == 1)
				res.push_back(s.substr(i - 9, 10));
		}
		return res;
	}
};
```

### 方法二：

　　用ACGT的ASC码的后3位来映射的解法。在代码编写中，对于最开始的9个字符是为了构造第一个key而做功，一般说来从第10个字符开始才开始判定key的重复出现情况。然而由于AGCT的映射码均为三位且没有任何一个码是000，因此在前面9个字符也是可以与其他字符一致处理的，不会出现误判。一致处理的代码大致如下：

```c++
class Solution {
public:
	vector<string> findRepeatedDnaSequences(string s) {
		vector<string> strs;
		unordered_map<int, int> map;
		int key = 0;
		for (int i = 0, end = s.size(); i < end; ++i) {
			key = ((key << 3) | (s[i] & 0x7)) & 0x3fffffff;
			int val = map[key]++;
			if (val == 1)
				strs.push_back(s.substr(i - 9, 10));
		}
		return strs;
	}
};
```

```c++
//不一致处理的代码大概如下，先处理前9个字符，然后处理后面的：
class Solution {
public:
    vector<string> findRepeatedDnaSequences(string s) {
        vector<string> strs;
        if (s.size() <= 10) return strs;
        unordered_map<int, int> map;
        int key = 0, i = 0; 
        for (; i < 9; ++i)
            key = ((key << 3) | (s[i] & 0x7)) & 0x3fffffff;
        for (int end = s.size(); i < end; ++i) {
            key = ((key << 3) | (s[i] & 0x7)) & 0x3fffffff;
            if (map[key]++ == 1)
                strs.push_back(s.substr(i - 9, 10));
        }
        return strs; 
    }
};
```

### 