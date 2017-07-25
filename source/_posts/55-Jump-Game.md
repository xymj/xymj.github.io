---
title: 55. Jump Game
date: 2017-07-25 21:29:45
tags: LeetCode
categories: LeetCode
---

# 55. Jump Game

Given an array of non-negative integers, you are initially positioned at the first index of the array.
Each element in the array represents your maximum jump length at that position.
Determine if you are able to reach the last index.

For example:
A = [2,3,1,1,4], return true.
A = [3,2,1,0,4], return false. 

<!--more-->

## 题意：

　　给定一组非负整数，首先定位在数组的第一个索引中。数组中的每个元素表示您在该位置的最大跳转长度。确定是否能够到达最后一个索引。

## 思路：

### 　方法一：

　　用一个变量来存储这个变量所能到达的数组的最远坐标maxReach = i + nums[i]，然后遍历数组下一个元素，比较下个元素的数组索引i与maxReach的大小，如果maxReach小于i,说明最远距离不能到达最近的节点，那么肯定不能到末尾，如果maxReach大于i,说明最远距离能到达最近的节点，然后再去这个最近节点的最远到达坐标maxReach,依次查找，当maxReach大于等于len-1的时候说明一定能到数组的末尾，确实是典型的贪心算法，总是找满足条件最好的结果，然后向前推进，直到找到结果。

```c++
class Solution {
public:
	bool canJump(vector<int>& nums) {
		int len = nums.size();
		int maxReach = 0;
		for (int i =0;i<len;i++)
		{
			if (maxReach<i)
			{
				return false;
			}
			if (maxReach>=len-1)
			{
				return true;
			}
			maxReach = max(maxReach, i + nums[i]);
		}
	}
};
```

### 　方法二：

　　和方法一类似，也是从前往后遍历数组，但是用一个变量来保存所能走的步数相对于当前数组元素坐标索引，每次遍历一个元素代表向前走一步step--，然后获取当前元素nums[i]的值和step的最大值，确定往后还能移动的最大步数，也是典型的贪心算法，不断的更新最远步长step，然后判断是否能满足到达末尾，最后step>=0肯定能走到末尾。

```c++
class Solution {
public:
	bool canJump(vector<int>& nums) {
		int len = nums.size();
		int step = nums[0];//初始化最初能走的最远步长
		for (int i = 1; i < len; i++)
		{
			step--;
			if (step<0)
			{
				return false;
			}
			step = max(step, nums[i]);
		}
		return step >= 0;
	}
};
```

### 　方法三：

　　从后往前遍历数组，判断前一个元素是否能到达它后面的元素，根据如果能到达，则往前遍历，把刚刚那个元素做为到达的目标点元素，在判断它之前的元素能否到达它，一次往前遍历，如果能走到第一个元素，说明能到达最后元素，否则不能。

```c++
class Solution {
public:
	bool canJump(vector<int>& nums) {
		int len = nums.size();
		int dest = len - 1;
		int cur_pos = len - 2;
		while (cur_pos>=0)
		{
			if (cur_pos+nums[cur_pos]>=dest)
			{
				dest = cur_pos;
			}
			cur_pos--;
		}
		return dest == 0;
	}
};
```

