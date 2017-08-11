---
title: 97. Interleaving String
date: 2017-08-11 22:55:43
tags: LeetCode
categories: LeetCode
---

# 97. Interleaving String

Given s1, s2, s3, find whether s3 is formed by the interleaving of s1 and s2.

For example,
Given:
s1 = "aabcc",
s2 = "dbbca",

When s3 = "aadbbcbcac", return true.
When s3 = "aadbbbaccc", return false. 

<!--more-->

## 题意：

　　给定S1、S2、S3，确定是否由S1和S2交织形成S3。

## 思路：

 　　动态规划的思想，分解成小问题，s3[0,i+j]的字符与s1[0,i],s2[0,j]匹配，s3[0,i+j+1]的状态取决于s3[i+j+1] 与 s1[i+1]或者s2[0,j+1]是否匹配

```cassandra
设dp[i][j]，表示s1[0,i-1]和s2[0,j-1]交替组成s3[0, i+j-1]。//dp[i][j]就表示s1的前i个和s2的前j个是否和s3的前i+j个匹配
如果s1[i-1]等于s3[i+j-1]，则dp[i][j]=dp[i-1][j]
如果s2[j-1]等于s3[i+j-1]，则dp[i][j]=dp[i][j-1]
因此状态转移方程如下：
dp[i][j] = dp[i-1][j] && (s1[i-1] == s3[i+j-1]) || dp[i][j-1] && (s2[j-1] == s3[i+j-1])
```

```c++
class Solution {
public:
	bool isInterleave(string s1, string s2, string s3) {
		int s1Len = s1.size(), s2Len = s2.size(), s3Len = s3.size();
		if (s3Len != s2Len + s1Len)
		{
			return false;
		}
		else if (s1Len == 0 && s2Len == 0 && s3Len == 0) return true;
		else if (s1[0] != s3[0] && s2[0] != s3[0]) return false;
      
		//vector<vector<bool>> dp(s1Len + 1, vector<bool>(s2Len + 1, false));//这样造成效率低
		vector<vector<int>> dp(s1Len + 1, vector<int>(s2Len + 1, 0));
		dp[0][0] = 1;
		for (int i = 1; i <= s1Len; i++)
		{
			dp[i][0] = (s1[i - 1] == s3[i - 1]) && dp[i - 1][0];
		}
		for (int j = 1; j <= s2Len; j++)
		{
			dp[0][j] = (s2[j - 1] == s3[j - 1]) && dp[0][j - 1];
		}
		for (int i = 1; i <= s1Len; i++)
		{
			for (int j = 1; j <= s2Len; j++)
			{
				int k = i + j;
				dp[i][j] = (s1[i - 1] == s3[k - 1]) && dp[i - 1][j] || (s2[j - 1] == s3[k - 1]) && dp[i][j - 1];
			}

		}
		return dp[s1Len][s2Len];
	}
};
```

