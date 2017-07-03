---
title: 12. Integer to Roman
date: 2017-07-03 20:18:09
tags: LeetCode
categories: LeetCode
---

# 12. Integer to Roman

Given an integer, convert it to a roman numeral.
Input is guaranteed to be within the range from 1 to 3999.

## 题意：

　　给定一个整数，将其转换为罗马数字，输入的数字确保在1到3999之间。

<!-- more -->

## 思路：

　　**罗马数字计数规则：**

1. 若干相同数字连写表示的数是这些罗马数字的和，如III = 3；
2. 小数字在大数字前面表示的数是用大数字减去小数字，如IV＝4；
3. 小数字在大数字后面表示的数是用大数字加上小数字，如VI = 6;

　　**罗马数字组合规则：**

1. 基本数字I、X 、C 中的任何一个，自身连用构成数目，或者放在大数的右边连用构成数目，都不能超过三个；放在大数的左边只能用一个。
2. 不能把基本数字 V 、L 、D 中的任何一个作为小数放在大数的左边采用相减的方法构成数目；放在大数的右边采用相加的方式构成数目，只能使用一个。
3. V 和 X 左边的小数字只能用I。
4. L 和 C 左边的小数字只能用×。
5. D 和 M 左 边的小数字只能用 C 。

　　**具体实现方法：**

1. 利用map构造哈希映射表，把1到1000的所有数字用罗马数字表示，这几个数字也就是构成罗马数字的基本元素，组合可以够造数字：

   map<char, int>  m = { { 'I',1 },{ 'V',5 },{ 'X',10 },{ 'L',50 },{ 'C',100 },{ 'D',500 },{ 'M',1000 } };

2. 利用整数数组和字符串数组构造哈希映射表：

   int val[] = { 1000,900,500,400,100,90,50,40,10,9,5,4,1 };

   string r[] = { "M","CM","D","CD","C","XC","L","XL","X","IX","V","IV","I" };

```c++
//方法一：
class Solution {
public:
	string intToRoman(int num) {
        map<int, string> mp = { { 1000,"M" },{ 900,"CM" },{ 500,"D" },{ 400,"CD" },{ 100,"C" },{ 90,"XC" },{ 50,"L" },{ 40,"XL" },{ 10,"X" },{ 9,"IX" },{ 5,"V" },{ 4,"IV" },{ 1,"I" } };
        map<int, string>::reverse_iterator iter = mp.rbegin();//map按键值顺序排列，所以取值的时候要利用逆序迭代器  
        string roman;
        while (num > 0&& iter!=mp.rend()) 
        {
            while (num >= iter->first) {
                roman += iter->second;
                num -= iter->first;
            }
            ++iter;
      	}
    	return roman;
	}
};

//方法二：
class Solution {
public:
      string intToRoman4(int num) {
          int n[] = { 1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1 };
          string r[] = { "M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I" };
          string roman;
          int i = 0;
          while (num > 0) {
              while (num >= n[i]) {
                  roman += r[i];
                  num -= n[i];
              }
              ++i;
          }
          return roman;
      }
};
```

