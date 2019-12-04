---
title: 51. N-Queens
date: 2017-07-22 15:46:11
tags: LeetCode
categories: LeetCode
---

# 51. N - Queens

The n-queens puzzle is the problem of placing n queens on an n×n chessboard such that no two queens attack each other.
Given an integer n, return all distinct solutions to the n-queens puzzle.
Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space respectively.

For example,
There exist two distinct solutions to the 4-queens puzzle:
[
　　[".Q..",  // Solution 1
　　"...Q",
　　"Q...",
　　"..Q."],

　　["..Q.",  // Solution 2
　　Q...",
　　"...Q",
　　".Q.."]
]

<!--more-->

## 题意：

　　n皇后难题的问题是将n个皇后放在在n×n棋盘，没有任何两个皇后互相攻击。给定一个整数n，返回所有不同的解决n皇后问题的解。每个解决方案包含一个独特的N皇后的位置板配置，分别用“Q”和“.”都表明一个皇后和一个空的空间。

## 思路：

　　求解N皇后问题是算法中回溯法应用的一个经典案例, 回溯算法也叫试探法，它是一种系统地搜索问题的解的方法。回溯算法的基本思想是：从一条路往前走，能进则进，不能进则退回来，换一条路再试。  
　　在现实中，有很多问题往往需要我们把其所有可能穷举出来，然后从中找出满足某种要求的可能或最优的情况，从而得到整个问题的解。回溯算法就是解决这种问题的“通用算法”，有“万能算法”之称。N皇后问题在N增大时就是这样一个解空间很大的问题，所以比较适合用这种方法求解。这也是N皇后问题的传统解法。




```markdown
下面是算法的高级伪码描述，这里用一个N*N的矩阵来存储棋盘：
   1) 算法开始, 清空棋盘，当前行设为第一行，当前列设为第一列
   2) 在当前行，当前列的位置上判断是否满足条件(即保证经过这一点的行,列与斜线上都没有两个皇后)，若不满足，跳到第4步
   3) 在当前位置上满足条件的情形：
      	在当前位置放一个皇后，若当前行是最后一行，记录一个解；
        若当前行不是最后一行，当前行设为下一行, 当前列设为当前行的第一个待测位置；
        若当前行是最后一行，当前列不是最后一列，当前列设为下一列；
        若当前行是最后一行，当前列是最后一列，回溯，即清空当前行及以下各行的棋盘，然后，当前行设为上一行，当前列设为当前行的下一个待测位置；
        以上返回到第2步
   4) 在当前位置上不满足条件的情形：
        若当前列不是最后一列，当前列设为下一列，返回到第2步;
        若当前列是最后一列了，回溯，即，若当前行已经是第一行了，算法退出，否则，清空当前行及以下各行的棋盘，然后，当前行设为上一行，当前列设为当前行的下一个待测位置，返回到第2步;

	算法的基本原理是上面这个样子，但不同的是用的数据结构不同，检查某个位置是否满足条件的方法也不同。为了提高效率，有各种优化策略，如多线程，多分配内存表示棋盘等。
	在具体解决该问题时，可以将其拆分为几个小问题。首先就是在棋盘上如何判断两个皇后是否能够相互攻击，使用一个一维数组来存储棋盘，在某个位置上是否有皇后可以相互攻击的判断也很简单。
具体细节如下：
   　把棋盘存储为一个N维数组a[N]，数组中第i个元素的值代表第i行的皇后位置，这样便可以把问题的空间规模压缩为一维O(N)，在判断是否冲突时也很简单，首先每行只有一个皇后，且在数组中只占
据一个元素的位置，行冲突就不存在了，其次是列冲突，判断一下是否有a[i]与当前要放置皇后的列j相等即可。至于斜线冲突，通过观察可以发现所有在斜线上冲突的皇后的位置都有规律即它们所在的行
列互减的绝对值相等，即| row – i | = | col – a[i] | 。这样某个位置是否可以放置皇后的问题已经解决。    
```



```c++
   //下面要解决的是使用何种方法来找到所有的N皇后的解。上面说过该问题是回溯法的经典应用，所以可以使用回溯法来解决该问题，具体实现也有两个途径，递归和非递归。递归方法较为简单，大致思想如下：
void queen(int row)
{
     if (n == row)      //如果已经找到结果，则打印结果
      	print_result();
     else
     {
       for (k=0 to N)
       { //试探第row行每一个列
         if (can_place(row, k)
         {
           place(row, k);   //放置皇后
           queen(row + 1);  //继续探测下一行
         }
        }
     }
}
//该方法由于在探测第i行后，如果找到一个可以放置皇后的位置j后，则会递归探测下一行，结束后则会继续探测i行j+1列，故可以找到所有的N皇后的解。
```
### 　方法一：

　　递归实现，主要思路如上述思路所述。

```c++
/****************************************************************************************************************
You are here! Your runtime beats 18.30% of cpp submissions.
9 / 9 test cases passed.
Status: Accepted
Runtime: 32 ms
****************************************************************************************************************/
class Solution {
public:
	vector<vector<string>> solveNQueens(int n) {
		vector<vector<string>> res;
		/*
		*使用一个一维数组表示皇后的位置
		* 其中数组的下标表示皇后所在的行
		* 数组元素的值表示皇后所在的列
		* 这样设计的棋盘，所有皇后必定不在同一行，于是行冲突就不存在了
		*/
		vector<int> check(n, 0);
		placeQueens(res, check, 0, n);
		return res;
	}
	void placeQueens(vector<vector<string>> &res,vector<int> check, int row,int n) {
		if (row>n-1)
		{
			string rowStr(n, '.');
			vector<string> tempQueens(n, rowStr);
			for (int i = 0;i<n;i++)
			{
				tempQueens[i][check[i]] = 'Q';
			}
			res.push_back(tempQueens);
			return;
		}
		for (int j = 0; j < n; j++)//控制每一行的列
		{
			if (checkPlace(row,j,check))//通过本行是否可以放皇后做为控制回溯的条件
			{
				check[row] = j;
				placeQueens(res, check, row + 1, n);//当前行找到合适解，递归进行下一行寻找皇后位置的解
			}
		}
	}
	int checkPlace(int row, int col,vector<int> check) {//检测第row行，第col列是否可以放皇后
		for (int i = 0;i<row;i++)
		{
			if (check[i]==col||abs(check[i]-col)==abs(row-i))
			{
				return 0;
			}
		}
		return 1;
	}
};
```

### 　方法二：

　　 一般来说递归的效率比较差，下面重点讨论一下该问题的非递归实现。非递归方法的一个重要问题时何时回溯及如何回溯的问题。程序首先对N行中的每一行进行探测，寻找该行中可以放置皇后的位置，具体方法是对该行的每一列进行探测，看是否可以放置皇后，如果可以，则在该列放置一个皇后，然后继续探测下一行的皇后位置。如果已经探测完所有的列都没有找到可以放置皇后的列，此时就应该回溯，把上一行皇后的位置往后移一列，如果上一行皇后移动后也找不到位置，则继续回溯直至某一行找到皇后的位置或回溯到第一行，如果第一行皇后也无法找到可以放置皇后的位置，则说明已经找到所有的解程序终止。如果该行已经是最后一行，则探测完该行后，如果找到放置皇后的位置，则说明找到一个结果，打印出来。但是此时并不能再此处结束程序，因为我们要找的是所有N皇后问题所有的解，此时应该清除该行的皇后，从当前放置皇后列数的下一列继续探测。

```c++
/****************************************************************************************************************
You are here! Your runtime beats 18.30% of cpp submissions.
9 / 9 test cases passed.
Status: Accepted
Runtime: 32 ms
****************************************************************************************************************/
class Solution {
public:
	vector<vector<string>> solveNQueens(int n) {
		vector<vector<string>> res;
		/*
		*使用一个一维数组表示皇后的位置
		* 其中数组的下标表示皇后所在的行
		* 数组元素的值表示皇后所在的列
		* 这样设计的棋盘，所有皇后必定不在同一行，于是行冲突就不存在了
		*/
		vector<int> check(n, INT_MIN);
		int i = 0, j = 0;
		while (i<n)
		{
			while (j<n)// 对i行的每一列进行探测，看是否可以放置皇后
			{
				if (checkPlace(check,i,j))//该位置可以放置皇后  
				{
					check[i] = j;//第i行放置皇后  
					j = 0;//第i行放置皇后以后，需要继续探测下一行的皇后位置，所以此处将j清零，从下一行的第0列开始逐列探测  
					break;
				}
				else
				{
					j++;//继续探测下一列
				}
			}
			if (check[i]==INT_MIN)//第i行没有找到可以放置皇后的位置  
			{
				if (i==0) // 回溯到第一行，仍然无法找到可以放置皇后的位置，则说明已经找到所有的解，程序终止
				{
					break;
				}
				else //没有找到可以放置皇后的列，此时就应该回溯
				{
					i--;
					j = check[i] + 1;//把上一行皇后的位置往后移一列  
					check[i] = INT_MIN;//把上一行皇后的位置清除，重新探测
					continue;
				}
			}
			if (i==n-1)  //最后一行找到了一个皇后位置，说明找到一个结果，打印出来  
			{
				string rowStr(n, '.');
				vector<string> tempQueens(n, rowStr);
				for (int i = 0; i < n; i++)
				{
					tempQueens[i][check[i]] = 'Q';
				}
				res.push_back(tempQueens);
				//不能在此处结束程序，因为我们要找的是N皇后问题的所有解，此时应该清除该行的皇后，从当前放置皇后列数的下一列继续探测。
				j = check[i] + 1;//从最后一行放置皇后列数的下一列继续探测  
				check[i] = INT_MIN;//清除最后一行的皇后位置  
				continue;
			}
			i++;  //继续探测下一行的皇后位置
		}
		return res;
	}
	int checkPlace(vector<int> check,int row, int col) {
		for (int i =0;i<row;i++)
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

### 　方法三：

　　递归实现，主要思路如上述思路所述，但是此方法是利用位运算之间的关系来判断皇后放的位置，效率高(但是自己也不太理解)。

```c++
/***************************************************************************************************************
You are here! Your runtime beats 48.15% of cpp submissions.
9 / 9 test cases passed.
Status: Accepted
Runtime: 9 ms
****************************************************************************************************************/
class Solution {
public:
	vector<vector<string>> res;
	int upperLimit = 1;
	vector<vector<string>> solveNQueens(int n){
		upperLimit = (upperLimit << n) - 1;
		vector<string> tempRes(n, string(n, '.'));
		placeQueens(0, 0, 0,tempRes,0);
		return res;
	}
	void placeQueens(int row, int lXie, int rXie,vector<string> tempRes,int lineNum) {
		if (row==upperLimit)
		{
			res.push_back(tempRes);
			return;
		}
		int n = tempRes.size();
		int pos = upperLimit&~(row | lXie | rXie);
		while (pos)
		{
			int p = pos&-pos;
			int tempP = p;
			int counts = 0;
			while (tempP)
			{
				tempP = tempP >> 1;
				counts++;
			}
			tempRes[lineNum][n - counts] = 'Q';
			pos -= p;
			placeQueens(row + p, (lXie + p) << 1, (rXie + p) >> 1, tempRes,lineNum+1);
			tempRes[lineNum][n - counts] = '.';
		}
	}
};
```

---------------------------------------------------
#### 　Java Code
```Java
class Solution {
    public List<List<String>> solveNQueens(int n) {

        List<List<String>> res = new ArrayList();
        if (n == 0) {
            return res;
        }

        int[] rowCheck = new int[n];//下标索引代表行，索引对应的值代表列

        placeQueens(res, n, 0, rowCheck);

        return res;
    }

    public void placeQueens(List<List<String>> res, int n, int row, int[] rowCheck) {
        if (row == n) {
            List<String> tmp = new ArrayList();
            //char[][] tmpRes = new char[n][n];
            char[] tmpRowRes = new char[n];
            Arrays.fill(tmpRowRes, '.');
            //Arrays.fill(tmpRes, tmpRowRes);//这种里面存的是tmpRowRes引用，后面对一行的修改会影响所有
            for (int i = 0;i < n;i++) {
               StringBuilder builder = new StringBuilder(new String(tmpRowRes));
               builder.replace(rowCheck[i], rowCheck[i] + 1, "Q");
               tmp.add(builder.toString());
            }

            res.add(tmp);
            return;

        }

        for (int j = 0;j < n;j++) {
            if (checkValidPosition(row, j, rowCheck)) {
                rowCheck[row] = j;
                placeQueens(res, n, row + 1, rowCheck);
            }
        }
    }

    public boolean checkValidPosition(int row, int col, int[] rowCheck) {
        for (int i = 0;i < row;i++) {
            if (rowCheck[i] == col //同一列有queen冲突
                || Math.abs(row - i) == Math.abs(rowCheck[i] - col)) { //对角线上有queen冲突
                    return false;
                }
        }
        return true;
    }
}
```
