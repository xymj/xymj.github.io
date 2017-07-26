---
title: 60. Permutation Sequence
date: 2017-07-26 22:43:37
tags: LeetCode
categories: LeetCode
---

# 60. Permutation Sequence

The set [1,2,3,…,n] contains a total of n! unique permutations.

By listing and labeling all of the permutations in order,
We get the following sequence (ie, for n = 3):

　　"123"
　　"132"
　　"213"
　　"231"
　　"312"
　　"321"
Given n and k, return the kth permutation sequence.

Note: Given n will be between 1 and 9 inclusive.

<!--more-->

## 题意：

　　集合[ 1,2,3，…，n ]共包含n!独特的排列。可以列举和标记所有的顺序排列，给出n和k，返回n的集合中第k个排列。n在1到9之间。

## 思路：

　　思路是这样的，比如当前长度是n，我们知道每个相同的起始元素对应(n-1)!个permutation，也就是(n-1)!个permutation后会换一个起始元素。因此，只要当前的k进行(n-1)!取余，得到的数字就是当前剩余数组的index，如此就可以得到对应的元素。如此递推直到数组中没有元素结束。实现中我们要维护一个数组来记录当前的元素，每次得到一个元素加入结果数组，然后从剩余数组中移除。
　　假设有四位数字{1， 2， 3， 4}，那么他们能够产生的排列数是什么呢？

```markdown
1 + {2, 3, 4}
2 + {1, 3, 4}
3 + {1, 2, 4}
4 + {1, 2, 3}
```
　　其实就是选定第一位数字后，其他剩下的数字进行排列组合，就能求出以该数字打头的所有排列组合。想必已经能发现一些规律了，我们干脆再举一个具体的例子，比如我们现在想要找第14个数，那么由于14 = 6 + 6 + 2。因此第一个数打头的是3，然后再求{1, 2, 4}中第二个排列组合数，答案是"142"。所以最终答案就是"3142"啦。
　　这里有一些问题是需要我们注意的：1）构造排列数从最高位开始，当选出一个数字后，就应当把这个数字erase掉，防止后面又出现；2）我们所要求的第k个数需要在每次循环中减去对应的值；3）注意程序中的数组是从0开始的，但题目的输入是从1开始计数的。

```c++
class Solution {
public:
	string getPermutation(int n, int k) {
		string res;
		vector<int> permutation(n + 1, 1);
		//1 构造1到n的排列数组
		int fac = 1;
		for (int i = 2; i < n; i++)
		{
			fac *= i;
		}
		//2 构造查找最高位的字符数组
		vector<char> vals = { '1','2','3','4','5','6','7','8','9' };
		int level = n - 1;
		k--;////vals数组从0开始计数，k所以要减1
		while (level >= 0)
		{
			int index = k / fac;//vals数组从0开始计数，k所以要减1，fac对应的是去掉最高位后后面n-1位的排列数
			res += vals[index];
			k %= fac;
			vals.erase(vals.begin() + index);
			if (level)
				fac = fac / level;

			level--;
		}
		return res;
	}
};
```