---
title: 93. Restore IP Addresses
date: 2017-08-11 21:30:50
tags: LeetCode
categories: LeetCode
---

# 93. Restore IP Addresses

Given a string containing only digits, restore it by returning all possible valid IP address combinations.

For example:
Given "25525511135",

return ["255.255.11.135", "255.255.111.35"]. (Order does not matter) 

<!--more-->

## 题意：

　　给定一个只包含数字的字符串，返回所有可能的有效IP地址组合。

## 思路：

### 　方法一：

　　IP地址的三种分类：A类地址：10.0.0.0～10.255.255.255 B类地址：172.16.0.0～172.31.255.255 C类地址：192.168.0.0～192.168.255.255

　　这道题的主要思路是分治，将问题的规模不断缩小。回溯中好多的题目都是使用这个思想来解题的。IP地址被三个”.”分割成了4个部分，这里我把每一个部分叫做一个段，依次为第0段，第1段，第2段，第3段。
　　下面以25525511135为例来分析。考虑第0段可能的取值，由于每一个段可以包含1位，2位或3位，那么第0段可能的取值是：
　　　　2
　　　　25
　　　　255
　　第0段取值以后后面的字符串需要被分割成3段，那么后面的字符能否被分割成3段呢？
　　第0段取上面的值后，剩余的部分依次为：
　　　　5525511135 ，字符串的长度为10，而3段最长也只能为9，所以第0段取2时没有对应的有效ip地址。
　　　　525511135 ，这里字符串的长度为9，即这个字符串还可能被分割成有效的3段。
　　　　25511135 ，这里字符串的长度为8，即这个字符串还可能被分割成有效的3段。
　　然后再对525511135和25511135求分割成3段的方法。这里问题规模就缩小了。需要被分割的段由4段变成了3了，字符串的长度也变短了。
递归退出的条件是遍历到字符串尾端并且字符串被分割成了4段。

**几点注意的地方**：

	1. 如果一个段包含3个字符，那么这三个字符不能大于”255”,这是ip地址的基本知识
	2. 如果一个字符包括2个或3个字符，它的第一个字符不能为0，即在验证字符串是否是数字的时候，要注意0的情况，001，010，03都是非法的。所以，如果第一位取出来是0，那么我们就判断字符串是否是"0"，不是的情况都是非法的
	3. 取字符串的时候，注意位数不够的问题，不仅<4, 而且<s.length()
	4. 注意截取子串substring的范围
	5. 不能忘记IP地址里面的 "."
	6. 到第4个Part的时候我们就可以整体验证剩下的所有字符串（因为第4个Part最后一定要取到结尾才是正确的字符串）
```c++
class Solution {
public:
	vector<string> restoreIpAddresses(string s) {
		vector<string> res;
		if (s.size()==0||s.size()>12||s.size()<4)
		{
			return res;
		}
		getIpAddress(s, "", res, 0);
		return res;
	}
	void getIpAddress(string s, string temp, vector<string> &res, int count) {
		if (count==3)
		{
			if (isValidIp(s))
			{
				res.push_back(temp + s);
				return;
			}
			else
				return;
		}
		for (int i = 1;i<4&&i<s.size();i++)//少限制条件i<s.size()特例 Last executed input : "1111"
		{
			string str = s.substr(0, i);
			if (isValidIp(str))//递归回溯前先判断，进行剪枝
			{
				getIpAddress(s.substr(i), temp + str + '.', res, count + 1);
			}
			
		}
	}
	bool isValidIp(string s) {
		if (s[0]=='0')//判断首位为0的话是否合法
		{
			return s.size() == 1;
		}
		else {
			int num = atoi(s.c_str());
			if (num <= 255)
			{
				return true;
			}
			else
				return false;
		}
	}
};
```

### 　方法二：

　　循环迭代求解，三重循环，遍历三个小数点的位置，对每个位置check一下即可。
　　**注意：**
　　　　stoi函数默认要求输入的参数字符串是符合int范围的[-2147483648, 2147483647]，否则会runtime error。
　　　　atoi函数则不做范围检查，若超过int范围，则显示-2147483648（溢出下界）或者2147483647（溢出上界）。

```c++
class Solution {
public:
	vector<string> restoreIpAddresses(string s) {
		vector<string> res;
		if (s.size() == 0 || s.size() > 12 || s.size() < 4)
		{
			return res;
		}
		for (int i =0;i<s.size();i++)
		{
			for (int j =i+1;j<s.size();j++)
			{
				for (int k = j+1;k<s.size()-1;k++)
				{
					string str1 = s.substr(0, i + 1);
					string str2 = s.substr(i+1, j-i);
					string str3 = s.substr(j+1, k-j);
					string str4 = s.substr(k+1);
					if (isValidIp(str1)&&isValidIp(str2)&&isValidIp(str3)&&isValidIp(str4))
					{
						string str = str1 + "." + str2 + "." + str3 + "." + str4;
						res.push_back(str);
					}
				}
			}
		}
		return res;
	}
	bool isValidIp(string s) {
		if (s[0] == '0')
		{
			return s.size() == 1;
		}
		else {
			int num = atoi(s.c_str());
			if (num <= 255)
			{
				return true;
			}
			else
				return false;
		}
	}
};
```

