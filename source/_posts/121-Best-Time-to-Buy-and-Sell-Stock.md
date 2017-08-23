---
title: 121. Best Time to Buy and Sell Stock
date: 2017-08-21 21:25:07
tags: LeetCode
categories: LeetCode
---

# 121. Best Time to Buy and Sell Stock

Say you have an array for which the ith element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Example 1:
Input: [7, 1, 5, 3, 6, 4]
Output: 5
max. difference = 6-1 = 5 (not 7-1 = 6, as selling price needs to be larger than buying price)

Example 2:
Input: [7, 6, 4, 3, 1]
Output: 0
In this case, no transaction is done, i.e. max profit = 0.

<!--more-->

## 题意：

　　一个数组第i个元素是表示股票在第i天的价格，只允许**最多完成一个交易**（即买入一次股票，卖出一次股票），设计一个找到最大收益的算法。

## 思路：

### 　方法一：

　　典型动态规划题，动态规划的思想重要一点就是利用以前已经计算出来的值，重点不在递归上，而在于重复利用之前计算的值，减少计算量，提高效率，递归是辅助计算工具。
　　这题的主要思路就是把每天的收益减去之前的最小买入的值，然后存在一个当天收益的数组中，更新数组中的值得时候要比较当天减去最小买入收益值与之前的收益大小，存入收益最大值,然后取出最大值就是股票最大收益。

```c++
class Solution {
public:
	int maxProfit(vector<int>& prices) {
		int len = prices.size();
		if (len==0)
		{
			return 0;
		}
		vector<int> profit(len,0);
		int minBuy = prices[0];
		for (int i = 1; i < len; i++)
		{
			minBuy = min(minBuy, prices[i]);
			profit[i] = max(profit[i - 1], prices[i] - minBuy);

		}
		return profit[len-1];
	}
};
```

### 　方法二：

　　同方法一，但是不用收益数组来记录最大收益，而是利用临时变量来记录，当收益大时，直接更新临时变量，最后临时变量中存储的就是收益最大值。

```c++
class Solution {
public:
	int maxProfit(vector<int>& prices) {
		int len = prices.size();
		if (len == 0)
		{
			return 0;
		}
		int maxRes = 0;
		int minBuy = prices[0];
		for (int i = 0; i < len; i++)
		{
			minBuy = min(minBuy, prices[i]);
			maxRes = max(maxRes, prices[i] - minBuy);

		}
		return maxRes;
	}
};
```

