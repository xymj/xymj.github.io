---
title: 123. Best Time to Buy and Sell Stock III
date: 2017-08-21 22:02:03
tags: LeetCode
categories: LeetCode
---

# 123. Best Time to Buy and Sell Stock III

Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete at most two transactions.

Note:
You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

<!--more-->

## 题意：

　　一个数组第i个元素是表示股票在第i天的价格，设计一个算法找到最大的利润。可以完成**两次交易**，即买卖股票两次。然而，你不可以从事多个事务在同一时间（即，你必须卖出股票，在你再次购买股票前）。

## 思路：

### 　方法一：

　　根据[121. Best Time to Buy and Sell Stock](http://blog.taoaili999.cn/2017/08/21/121-Best-Time-to-Buy-and-Sell-Stock/)的思路,构造一个从前往后买的最大收益数组，在构造一个从后往前买的最大收益数组，刚好满足两次交易。

```c++
不同于Best Time to Buy and Sell Stock I中定义的初始状态A[i]表示第i天卖出挣的最大数目的钱，这个更进一步直接定义A[i]表示前i天赚的最大数目的钱。minPrice表示从第0天到第i-1天中的最低价格。
A[0]=0。（初始状态）
A[1]=max(prices[1]-prices[0],A[0])
A[2]=max(prices[2]-minPrice,A[1])
.....
即A[i]=max(price[i]-minPrice,A[i-1]).
A[0]=0
  
另外一次扫描从数组后向前扫描，定义B[i]表示从第i天到最后一天n能赚的最大数目的钱。maxPrice表示第i+1天到n天的最高价格。
B[n]=0。（初始状态）
B[n-1]=max(maxPrice-prices[n-1],B[n])
B[n-2]=max(maxPrice-prices[n-2],B[n-1])
.....
即B[i]=max(maxPrice-prices[i],B[i+1])
B[n]=0
  
那么以第i天为分割点能赚的最多数目的钱为A[i]+B[i]
问题的解为max{A[i]+B[i]}。0<=i<=n。
时间复杂度是O(N)，空间复杂度是O(N)。
```

```c++
class Solution {
public:
	int maxProfit(vector<int>& prices) {
		int res = 0;
		int len = prices.size();
		if (len==0)
		{
			return res;
		}
		int minBuyPre = prices[0];
		vector<int> preProfit(len, 0);
		for (int i =1;i<len;i++)
		{
			minBuyPre = min(minBuyPre, prices[i]);
			preProfit[i] = max(prices[i] - minBuyPre, preProfit[i - 1]);
		}
		int maxSailTail = prices[len-1];
		vector<int> tailProfit(len, 0);
		for (int i =len-2;i>=0;i--)
		{
			maxSailTail = max(maxSailTail, prices[i]);
			tailProfit[i] = max(maxSailTail-prices[i], tailProfit[i + 1]);
		}
		for (int j = 0;j<len;j++)
		{
			res = max(preProfit[j] + tailProfit[j], res);
		}
		return res;
	}
};
```

### 　方法二：

　　在Discuss中看到一种很棒的解法，代码只有10行左右，但是不是很好理解。
　　第二种解法的核心是假设手上最开始只有0元钱，那么如果买入股票的价格为price，手上的钱需要减去这个price，如果卖出股票的价格为price，手上的钱需要加上这个price。

```c++
它定义了4个状态：
Buy1[i]表示前i天做第一笔交易买入股票后剩下的最多的钱；
Sell1[i]表示前i天做第一笔交易卖出股票后剩下的最多的钱；
Buy2[i]表示前i天做第二笔交易买入股票后剩下的最多的钱；
Sell2[i]表示前i天做第二笔交易卖出股票后剩下的最多的钱；

那么Sell2[i]=max{Sell2[i-1],Buy2[i-1]+prices[i]}
Buy2[i]=max{Buy2[i-1],Sell[i-1]-prices[i]}
Sell1[i]=max{Sell[i-1],Buy1[i-1]+prices[i]}
Buy1[i]=max{Buy[i-1],-prices[i]}
```
　　可以发现上面四个状态都是只与前一个状态有关，所以可以不使用数组而是使用变量来存储即可。这是leetcode中的讨论网址：https://leetcode.com/discuss/18330/is-it-best-solution-with-o-n-o-1

```c++
class Solution {
public:
	int maxProfit(vector<int>& prices) {
		int len = prices.size();
		int buy1 = INT_MIN;
		int buy2 = INT_MIN;
		int sell1 = 0, sell2 = 0;
	    for (int i = 0;i<len;i++)
	    {
			sell2 = max(sell2, prices[i] + buy2);
			buy2 = max(buy2, sell1-prices[i]);
			sell1 = max(sell1, prices[i] + buy1);
			buy1 = max(buy1, -prices[i]);
	    }
		return sell2;
	}
};
```





