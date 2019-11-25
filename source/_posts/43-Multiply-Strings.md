---
title: 43. Multiply Strings
date: 2017-07-20 21:46:48
tags: LeetCode
categories: LeetCode
---

# 43. Multiply Strings

Given two numbers represented as strings, return multiplication of the numbers as a string.

Note:
　　The numbers can be arbitrarily large and are non-negative.
　　Converting the input string to integer is NOT allowed.
　　You should NOT use internal library such as BigInteger.

<!--more-->

## 题意：

　　给定两个数值用string类型表示，返回这个两个数字的乘积，返回值也用string类型表示。
　　**注意：**
　　　　这些数字可以是任意大的，但不是负数。
　　　　不允许将输入字符串转换为整数。
　　　　你不应该使用内部类型如BigInteger。

## 思路：

　　 按照乘法的基本运算规律，把每一位的乘积累加和计算出来以后，再对每一位考虑进位问题。这道题的要求是计算大数乘法。其中大数是以字符串的形式表示，任意大，非负，返回结果以字符串形式。
　　假设两个整数的长度分别为了l1和l2，则其最后结果长度为l1+l2（最后有进位）或者l1+l2-1（最后没有有进位）。因此，可以先用长度为l1+l2的数组记录结果，最后再转成字符串。
　　进行乘法的时候，先把各个位的相乘结果对应累加起来，即第1个整数的第i位（低位到高位）和第2个整数的第j位（低位到高位）相乘的结果应该存放在数组的i+j位。然后再统一处理进位。然后再统一处理进位。最后再将数组转成字符串前，需要跳过前面的零。如果结果只有0，则只返回0。

　　时间复杂度：O(l1l2)（l1和l2分别为两个整数长度）
　　空间复杂度：O(l1+l2)

```c++
class Solution {
public:
	string multiply(string num1, string num2) {
		string res = "";
		int num1Len = num1.size();
		int num2Len = num2.size();
		if (num1Len == 0 || num2Len == 0)
		{
			return res;
		}
		if (num1 == "0" || num2 == "0")
			return "0";
		vector<int> bitSum(num1Len + num2Len, 0);
		for (int i =0;i<num2Len;i++)
		{
			for (int j = 0;j<num1Len;j++)//第二个字符串的第i位乘第一个字符串的0~j位，结果放到i+j位置，并累加
			{
				bitSum[i + j] += (num1[num1Len - j - 1] - '0')*(num2[num2Len - i - 1] - '0');
			}
		}
		for (int i = 0,c=0;i<bitSum.size();i++)//处理进位
		{
			int num = bitSum[i] + c;
			bitSum[i] = num % 10;
			c = num / 10;
		}
		for (int i = bitSum.size()-1; i>=0; i--)//转换为字符串
		{
			res += (bitSum[i]+'0');
		}
		int index = 0;
		for (;index<res.size()-1;index++)//去除字符串前置0
		{
			if (res[index]=='0')
			{
				continue;
			}
			else
				break;
		}
		return res.substr(index);
	}
};

```

---------------------------------------------------
#### 　Java Code：
```Java
class Solution {
    public String multiply(String num1, String num2) {
        if (null == num1 || null == num2) {
            return "";
        }

        int len1 = num1.length();
        int len2 = num2.length();
        if (len1 == 0 || len2 == 0) {
            return "";
        }

        int resLen = len1 + len2;
        int[] resArray = new int[resLen];
        for (int i = 0; i < len2; i++) {
            int v2 = num2.charAt(len2 - i - 1) - '0';
            for (int j = 0; j < len1; j++) {
                int v1 = num1.charAt(len1 - j - 1) - '0';
                resArray[i + j] +=  v2 * v1;
            }
        }
        if ("0".equals(num1) || "0".equals(num2)) {
            return "0";
        }

        int carry = 0;
        for (int k = 0; k < resLen; k++) {
            int bitSum = carry + resArray[k];
            resArray[k] = bitSum % 10;
            carry = bitSum / 10;
        }

        StringBuilder builder = new StringBuilder();
        int tail = resLen - 1;
        while (tail >= 0) {
            if (resArray[tail] != 0) {
                break;
            }
            tail--;
        }
        while (tail >= 0) {
            builder.append(resArray[tail]);
            tail--;
        }
        return builder.toString();
    }
}
```
