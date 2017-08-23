---
title: 122. Best Time to Buy and Sell Stock II
date: 2017-08-21 21:42:24
tags: LeetCode
categories: LeetCode
---

# 122. Best Time to Buy and Sell Stock II

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times). However, you may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

<!--more-->

## 题意：

　　一个数组第i个元素是表示股票在第i天的价格，设计一个算法找到最大的利润。可以完成**多次交易**，即买卖股票多次。然而，你不可以从事多个事务在同一时间（即，你必须卖出股票，在你再次购买股票前）。

## 思路：

### 　方法一：

　　在Best Time to Buy and Sell Stock系列中，和本题最像的是[[188. Best Time to Buy and Sell Stock IV]() ，
在Best Time to Buy and Sell Stock IV中是求某个给定k次交易的最大收益，本题由于是可以操作任意次数，只为获得最大收益，而且对于一个上升子序列，比如：[5, 1, 2, 3, 4]中的1, 2, 3, 4序列来说，对于两种操作方案：

1. 在1买入，4卖出
2. 在1买入，2卖出同时买入，3卖出同时买入，4卖出

这两种操作下，收益是一样的。
所以可以从头到尾扫描prices，如果price[i] – price[i-1]大于零则计入最后的收益中，即贪心法。

```c++
class Solution {
public:
	int maxProfit(vector<int>& prices) {
		int res = 0;
		int len = prices.size();
		if (len == 0)
			return 0;
		for (int i = 1; i < len; i++) {
			res += max(prices[i] - prices[i - 1], 0);
		}
		return res;
	}
};
```

### 　方法二：

　　根据[121. Best Time to Buy and Sell Stock]的思路,构建利益数组，当到某天价格减去之前购买最小值的结果大于当天利益收入时，更新最小购买值的值，寻找下次抛出的最大值，一直到最后，利用贪心的策略，即利益一旦减小，就记录利益的最大值，更新后面最小值。

```c++
class Solution {
public:
	int maxProfit(vector<int>& prices) {
		int res;
		int len = prices.size();
		if (len == 0)
			return 0;
		vector<int> profit(len, 0);
		int minBuy = INT_MAX;
		for (int i = 0; i < len; i++) {
			minBuy = min(prices[i], minBuy);
			int tempVal = prices[i] - minBuy;
			if (tempVal > profit[i])
			{
				profit[i] = tempVal;
				minBuy = prices[i];
			}	
		}
		for (int s : profit)
		{
			res += s;
		}
		return res;
	}
};
```

