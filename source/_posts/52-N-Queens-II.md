---
title: 52. N-Queens II
date: 2017-07-24 22:54:59
tags: LeetCode
categories: LeetCode
---

# 52. N - Queens II

Follow up for N-Queens problem.

![52-8-queens](images/52-8-queens.png)

Now, instead outputting board configurations, return the total number of distinct solutions.

<!--more-->

## 题意：

　　跟进的N皇后问题。现在，N皇后的输出板配置，返回不同解决方案的总数。

## 思路：

　　此题其实和[51. N - Queens](http://blog.taoaili999.cn/2017/07/22/51-N-Queens/)是同一个题，不同的地方是[51. N - Queens](http://blog.taoaili999.cn/2017/07/22/51-N-Queens/)是求所有的结果集返回，此题要求的是求得结果集的个数，也有三种方法，具体思路见[51. N - Queens](http://blog.taoaili999.cn/2017/07/22/51-N-Queens/)。

### 　方法一：

　　递归回溯，同[51. N - Queens](http://blog.taoaili999.cn/2017/07/22/51-N-Queens/)方法一。

```c++
class Solution {
public:
	int totalNQueens(int n) {
		vector<int> check(n, 0);
		int counts = 0;//用来记录结果集的个数
		placeQueens(counts, check, 0, n);
		return counts;
	}
	void placeQueens(int &counts, vector<int> check,int row, int n) {
		if (row>n-1)
		{
			counts++;
			return;
		}
		for (int j = 0;j<n;j++)
		{
			if (placeIsValid(check,row,j))
			{
				check[row] = j;
				placeQueens(counts, check, row + 1, n);
			}
		}
	}
	int placeIsValid(vector<int> check, int row, int col) {
		for (int i = 0; i < row; i++)
		{
			if (check[i]==col||abs(check[i]-col)==abs(i-row))
			{
				return 0;
			}
		}
		return 1;
	}
};
```

### 　方法二：

　　非递归实现，同[51. N - Queens](http://blog.taoaili999.cn/2017/07/22/51-N-Queens/)方法二。

```c++
class Solution3 {
public:
	int totalNQueens(int n) {
		vector<int> check(n, INT_MIN);
		int counts = 0;
		int i = 0, j = 0;
		while (i<n)
		{
			while (j<n)
			{
				if (placeIsValid(check,i,j))
				{
					check[i] = j;
					j = 0;//第i行放置皇后以后，需要继续探测下一行的皇后位置，所以此处将j清零，从下一行的第0列开始逐列探测  !!!!!!!!!!!!!!!!!
					break;
				}
				else
				{
					j++;
				}
			}
			if (check[i]==INT_MIN)
			{
				if (i==0)
				{
					break;
				}
				else
				{
					i--;
					j = check[i] + 1;
					check[i] = INT_MIN;
					continue;
				}
			}
			if (i == n-1)
			{
				counts++;

				j = check[i] + 1;
				check[i] = INT_MIN;
				continue;
			}
			i++;
		}
		return counts;
	}
	int placeIsValid(vector<int> check, int row, int col) {
		for (int i = 0; i < row; i++)
		{
			if (check[i] == col || abs(check[i] - col) == abs(i - row))
			{
				return 0;
			}
		}
		return 1;
	}
};
```

### 　方法三：

　　递归回溯位运算实现，同[51. N - Queens](http://blog.taoaili999.cn/2017/07/22/51-N-Queens/)方法三。

```c++
class Solution {
public:
	int counts = 0;
	int upperLimit = 1;
 	int totalNQueens(int n) {
		upperLimit = (upperLimit << n) - 1;
		placeQueens(0, 0, 0);
		return counts;
	}
	void placeQueens(int row, int lXie, int rXie)
	{
		if (row==upperLimit)//row是记录每一行的列位置皇后，当等于upperLimit最大限制的时候说明找到一个满足条件的皇后矩阵
		{
			counts++;
			return;
		}
		//~按位取反    ^按位异或
		int pos = upperLimit&~(row | lXie | rXie);//获取此时行的所有可插入皇后的位置
		while (pos)//pos不为0说明肯定有位置可以放皇后
		{
			//这个地方思想是按位取反加1，然后为了取到最右面的1必须与原pos相与，如果按自己下面的想法没有相与，导致出错
			//int p = pos;
			//p = ~p + 1;//先确定能放在此行的最右面的皇后的位置p
			//可以改为形式1
			int p = pos & (~pos + 1);

			// 拷贝pos最右边为1的bit，其余bit置0  
			// 也就是取得可以放皇后的最右边的列  
			//可以改为形式2
			/*pos & -pos 的意思就是取最右边的 1 再组成二进制数，相当于 pos &（~pos + 1），因为取反以后刚好所有数都是相反的（怎么听着像废话），再加 1 ，就是改变最低位，如果低位的几个数都是1，加的这个 1 就会进上去，一直进到 0 ，在做与运算就和原数对应的 1 重合了。举例可以说明：

				原数 0 0 0 0 1 0 0 0    原数 0 1 0 1 0 0 1 1
				取反 1 1 1 1 0 1 1 1    取反 1 0 1 0 1 1 0 0
				加1    1 1 1 1 1 0 0 0    加1  1 0 1 0 1 1 0 1
				与运算    0 0 0 0 1 0 0 0 and 0 0 0 0 0 0 0 1
				其中呢，这个取反再加 1 就是补码，and 运算 与负数，就是按位和补码与运算。*/
			//long p = pos & -pos;   //p记录的是可以放皇后的最右面的一个                                             

								   // 将pos最右边为1的bit清零  
								   // 也就是为获取下一次的最右可用列使用做准备，  
								   // 程序将来会回溯到这个位置继续试探  
			pos -= p;         //去掉最右面一个选取的位置，进行递归判断下一个最右面皇后的位置        

			placeQueens(row + p, (lXie + p) << 1, (rXie + p) >> 1);
		}
	}
};
```

