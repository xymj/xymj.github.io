---
title: 77. Combinations
date: 2017-08-05 23:15:03
tags: LeetCode
categories: LeetCode
---

# 77. Combinations

Given two integers n and k, return all possible combinations of k numbers out of 1 ... n.

For example,
If n = 4 and k = 2, a solution is:
[
　　[2,4],
　　[3,4],
　　[2,3],
　　[1,2],
　　[1,3],
　　[1,4],
]

<!--more-->

## 题意：

　　给定两个整数n和k，从1到n的所有数中返回k个数的所有可能组合。即从n个树种取出k个数，找出所有的取出可能。

## 思路：

### 　方法一：

　　穷举出所有的可能，能想到用回溯递归法，找到所有满足条件的结果，不满足递归回溯，满足放入结果集，进行下一轮递归。

```c++
/****************************************************************************************************************
You are here! Your runtime beats 9.26% of cpp submissions.
27 / 27 test cases passed.
Status: Accepted
Runtime: 496 ms
此法的时间复杂度过高，因为for循环中的递归循环次数控制太过宽泛，造成循环次数过多，时间复杂度过高。
****************************************************************************************************************/
class Solution {
public:
	vector<vector<int>> res;
	vector<vector<int>> combine(int n, int k) {
		vector<int> tempRes;
		backtracking(tempRes, 1,0, k, n);//从1到n
		return res;
	}
	void backtracking(vector<int> tempRes, int index,int counts, int k, int n) {
		if (counts==k)//找到k个数，满足条件，返回结果集
		{
			res.push_back(tempRes);
			return;
		}
		for (int i = index;i<=n;i++)
		{
			tempRes.push_back(i);
			backtracking(tempRes, i + 1, counts + 1, k, n);
			tempRes.pop_back();
		}
	}
};
```

### 　方法二：

　　也是回溯递归法，但是此法代码效率高，效率高的主要原因就是方法一的递归循环中的控制条件考虑的不够细，不够到位，没有利用k这个元素个数来限制和降低循环次数。

```c++
/****************************************************************************************************************
You are here! Your runtime beats 67.65% of cpp submissions.
27 / 27 test cases passed.
Status: Accepted
Runtime: 103 ms
****************************************************************************************************************/
class Solution {
public:
	vector<vector<int>> res;
	vector<vector<int>> combine(int n, int k) {
		vector<int> tempRes;
		backtracking(tempRes, 1, k, n);
		return res;
	}
	void backtracking(vector<int> tempRes, int index, int k, int n) {
		if (0 == k)//k为0说明k个元素全部取到
		{
			res.push_back(tempRes);
			return;
		}
		for (int i = index; i <= n-k+1; i++)//k是要查找的元素个数，要保证找到k个元素，i肯定要小于n-k+1,即n个元素后面出去k个后的最大的元素
		{
			tempRes.push_back(i);
			backtracking(tempRes, i + 1, k-1, n);//此处是k-1,每递归下一层，所要取的元素个数都会较少1个，for循环的次数就会通过k改变而改变
			tempRes.pop_back();
		}
	}
};
```

### 　方法三：

　　和方法二相同，但是不是往vector中push_back()和pop_back()元素，利用数组下标索引，覆盖相应索引对应元素，减少vector的push_back()和pop_back()元素的时间消耗。所以此法代码效率由高一些。

```c++
/****************************************************************************************************************
You are here! Your runtime beats 92.37% of cpp submissions.
27 / 27 test cases passed.
Status: Accepted
Runtime: 69 ms
****************************************************************************************************************/
class Solution {
public:
	vector<vector<int>> res;
	vector<vector<int>> combine(int n, int k) {
		vector<int> tempRes;
		tempRes.resize(k);//设置数组大小，根据k进行设置大小，可以根据k值来变化索引值
		backtracking(tempRes, 1, k, n);
		return res;
	}
	void backtracking(vector<int> tempRes, int index, int k, int n) {
		if (0 == k)
		{
			res.push_back(tempRes);
			return;
		}
		for (int i = index; i <= n - k + 1; i++)//k是要查找的元素个数，要保证找到k个元素，i肯定要小于n-k+1,即n个元素后面出去k个后的最大的元素
		{
			tempRes[tempRes.size() - k] = i;//直接根据索引进行覆盖，完成本层元素值的存储，当返回上一层的时候，k值变换，又可以对应不同的索引值，进行值的覆盖，减少push_back()和pop_back()操作的时间消耗
			backtracking(tempRes, i + 1, k - 1, n);
			//tempRes.pop_back();
		}
	}
};
```

### 　方法四：

　　非递归回溯法实现，即通过循环迭代法，但是效率不是很好。

```c++
/****************************************************************************************************************
You are here!   Your runtime beats 32.68% of cpp submissions.
27 / 27 test cases passed.
Status: Accepted
Runtime: 129 ms
这是一种迭代非递归的方法实现，但是还是没有方法三的效率高
****************************************************************************************************************/
class Solution {
public:
	vector<vector<int>> combine(int n, int k) {
		vector<vector<int>> res;
		vector<int> tempRes(k,0);
		int i = 0;
		while (i >= 0&&tempRes[0]<=n-k+1)
		{
			tempRes[i]++;
			if (tempRes[i]>n)
			{
				i--;
			}
			else if (i == k-1)
			{
				res.push_back(tempRes);
			}
			else
			{
				i++;
				tempRes[i] = tempRes[i - 1];
			}
		}
		return res;
	}

};
```

---------------------------------------------------
### 　Java Code
```Java
class Solution {
    public List<List<Integer>> combine(int n, int k) {
       if (n == 0 || k == 0) {
           return new ArrayList();
       }
       List<List<Integer>> res = new ArrayList();

       getCombine(n, 0, k, new Integer[k], res);

       return res;
    }

    public void getCombine(int n, int index, int k, Integer[] tmpRes, List<List<Integer>> res) {
        if (0 == k) {
            //如果tmpRes 类型是int[],那么Arrays.asList(Arrays.copyOf(tmpRes, tmpRes.length) 结果是List<int[]> ，不能满足结果要求，注意Arrays.asList和Arrays.copyOf的用法。
            List<Integer> tmp = Arrays.asList(Arrays.copyOf(tmpRes, tmpRes.length));
            res.add(tmp);
            return;
        }
        for (int i = index;i < n - k + 1;i++) {
            tmpRes[tmpRes.length - k] = i + 1;
            getCombine(n, i + 1, k - 1, tmpRes, res);
        }
    }
}
```

```Java
class Solution {
    public List<List<Integer>> combine(int n, int k) {
       if (n == 0 || k == 0) {
           return new ArrayList();
       }
       List<List<Integer>> res = new ArrayList();
       List<Integer> tmpRes = new ArrayList();

       getCombine(n, 0, k, tmpRes, res);

       return res;
    }

    public void getCombine(int n, int index, int k, List<Integer> tmpRes, List<List<Integer>> res) {
        if (tmpRes.size() == k) {
            res.add(tmpRes);
            return;
        }
        for (int i = index;i < n;i++) {
            List<Integer> tmp = new ArrayList(tmpRes);
            tmp.add(i + 1);
            getCombine(n, i + 1, k, tmp, res);
        }
    }
}
```
