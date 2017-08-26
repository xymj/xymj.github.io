---
title: 188. Best Time to Buy and Sell Stock IV
date: 2017-08-26 17:17:29
tags: LeetCode
categories: LeetCode
---

Say you have an array for which the ith element is the price of a given stock on day i.
Design an algorithm to find the maximum profit. You may complete at most k transactions.

Note:
You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

<!--more-->

## 题意：

　　一个数组第i个元素是表示股票在第i天的价格，只允许**最多完成k次交易**，设计一个找到最大收益的算法。
　　**注意：**不能从事多个交易在同一时间（即，你必须卖出股票后，才能再次购买股票）。

## 思路：

　　典型的动态规划股票问题，这应该是股票问题中最难的，完成k次交易，获得最大收益。思路同[121. Best Time to Buy and Sell Stock](http://blog.taoaili999.cn/2017/08/21/121-Best-Time-to-Buy-and-Sell-Stock/)，[122. Best Time to Buy and Sell Stock II](http://blog.taoaili999.cn/2017/08/21/122-Best-Time-to-Buy-and-Sell-Stock-II/)，[123. Best Time to Buy and Sell Stock III](http://blog.taoaili999.cn/2017/08/21/123-Best-Time-to-Buy-and-Sell-Stock-III/)，利用动态规划的局部最优构造全局最优的思想解决问题。

### 方法一：

```c++
题目的关键是下面的动态转移方程:
local[i][j]=max(global[i-1][j-1]+max(diff,0),local[i-1][j]+diff)，
global[i][j]=max(local[i][j],global[i-1][j])，

而这个方程的具体理解如下：

首先global比较简单，不过是不断地和已经计算出的local进行比较，把大的保存在global中。

然后看local,关键是要理解local的定义，local[i][j]表示，前i天进行了j次交易，并且第i天进行了第j次交易的最大利润，所以local[i][j]中必然有一次交易，也就是当近一次交易，发生在第i天。 local由两个部分的比较完成。

第一部分是，global[i-1][j-1]+max(diff,0), 表示的就是，前面把之前的j - 1次交易，放在之前的i - 1天，然后让第i天来进行第j次交易，那么加入此时diff(price[i] - price[i - 1])大于零，那么正好可以可借助这次交易的机会增长里利润(利润= diff)，否则的话，如果diff小于零，那就在第i天当天进行一次买卖，凑一次交易的次数，但是产生利润为0.

第二部分是, local[i-1][j]+diff， 这里的 local[i-1][j]表示的是，前面j次交易在第i -1天就已经完成了，可是因为说了local[a][b]一定要表达在第a天完成了b次交易的最大利润，所以就需要强制使得交易在第i天发生，为了实现这一点，只需要在local[i - 1][j]的基础上，加上diff(price[i] - price[i - 1])就可以了。如果diff < 0 那也没有办法，因为必须满足local的定义。接下来算global的时候，总会保证取得一个更大的值。
```

```c++
这一题的难度要远高于前面几题，需要用到动态规划，但是需要额外的辅助。
先按照之前的方法对数组进行统计，计算出无限制条件下的最少交易次数tradeCount和最大获益profitCount。如果这个最少交易次数已经小于k了，那么直接返回最大获益即可。同时也因为在k < tradeCount的情况下，进行动态规划的效率很低，所以要先进行处理来避免。

在动态规划的部分，维护两个数组：local和global。其中local[i][j]表示总交易次数为i截止到第j天并且在最后一天要做交易的情况下的最大获益，global[i][j]表示总交易次数为i截止到第j天的最大获益。

之所以在global之外还要维护一个local数组，是因为在计算global[i][j]时，面临两种情况：

    最后一天不做交易，那么直接等于global[i][j - 1]
    最后一天要做交易，那么又需要分别考虑罪有一天是否有收益的问题，所以要增加一个local数组进行辅助

递推公式：
int diff = prices[j] - prices[j - 1];
local[i][j] = Math.max(global[i - 1][j - 1], local[i][j - 1] + diff);
global[i][j] = Math.max(global[i][j - 1], local[i][j]);

解释一下local[i][j] = Math.max(global[i - 1][j - 1], local[i][j - 1] + diff);这一条，当diff < 0时，在最后一条做交易必然是亏的，所以其实此时local[i][j]直接等于global[i - 1][j - 1]；当diff > 0时，本来应该比较两种情况的，global[i - 1][j - 1] + diff和local[i][j - 1] + diff，但是通过以下推断我们可以知道local[i][j - 1] > global[i - 1][j - 1]，所以无须比较。

推断：

因为global[i - 1][j - 1] = Math.max(global[i - 1][j - 2], local[i - 1][j - 1])
所以global[i - 1][j - 1] = global[i - 1][j - 2]或者global[i - 1][j - 1] = local[i - 1][j - 1])

由题意可知：local[i][j - 1] > local[i - 1][j - 1])

又因为local[i][j - 1] = Math.max(global[i - 1][j - 2], local[i][j - 2] + diff)
所以local[i][j - 1] >= global[i - 1][j - 2]

综上local[i][j - 1] > local[i - 1][j - 1])并且local[i][j - 1] >= global[i - 1][j - 2]，即local[i][j - 1] > global[i - 1][j - 1]

这里还有一个性质，就是当i大于最大收益所需的交易次数时，其实local[i][j] == global[i][j]，多出来的交易都是当天买卖，不会产生收益。
```

```c++
//“局部最优和全局最优解法”
class Solution {
public:
	int maxProfit(int k, vector<int>& prices) {
		int res = 0;
		int len = prices.size();
		if (k==0||len<2)
		{
			return 0;
		}
		if (k>=len/2)//k大于最多交易次数就变为122. Best Time to Buy and Sell Stock II
		{
			for (int i = 1;i<len;i++)
			{
				res += max(0, prices[i] - prices[i - 1]);
			}
			return res;
		}
		vector<vector<int>> local(len, vector<int>(k + 1, 0));
		vector<vector<int>> global(len, vector<int>(k + 1, 0));
		for (int j = 1; j <= k; j++)
		{
			for (int i = 1;i<len;i++)//注意动态方程中天数和交易次数在二维数组中行列顺序
			{
				/*int diff = prices[j] - prices[j - 1];
				local[i][j] = Math.max(global[i - 1][j - 1], local[i][j - 1] + diff);
				global[i][j] = Math.max(global[i][j - 1], local[i][j]);*/
				int diff = prices[i] - prices[i - 1];
				//local[i][j] = max(global[i - 1][j - 1] + max(diff, 0), local[i-1][j] + diff);
				local[i][j] = max(global[i - 1][j - 1], local[i-1][j] + diff);//此处local[i][j]表示总交易次数为i截止到第j天并且在最后一天要做交易的情况下的最大获益，global[i][j]表示总交易次数为i截止到第j天的最大获益。i表示交易次数，j表示第几天。
				global[i][j] = max(global[i-1][j], local[i][j]);
			}
		}
		return global[len - 1][k];
	}
};
```

### 方法二：

　　不太懂的一个方法,思路同[123. Best Time to Buy and Sell Stock III](http://blog.taoaili999.cn/2017/08/21/123-Best-Time-to-Buy-and-Sell-Stock-III/)方法二，代码简洁，但是不太理解。

```c++
class Solution {
public:
	int maxProfit(int k, vector<int>& prices) {
		int res = 0;
		int len = prices.size();
		if (k == 0 || len<2)
		{
			return 0;
		}
		if (k >= len / 2)
		{
			for (int i = 1; i<len; i++)
			{
				res += max(0, prices[i] - prices[i - 1]);
			}
			return res;
		}
		vector<int> buy(k+1,INT_MIN),sale(k+1,0);
		
		for (int i = 0;i<len;i++)
		{
			for (int j =1;j<=k;j++)
			{
				buy[j] = max(buy[j], sale[j - 1] - prices[i]);
				sale[j] = max(sale[j], buy[j] + prices[i]);
			}
		}
		return sale[k];
	}
};
```