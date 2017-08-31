---
title: 209. Minimum Size Subarray Sum
date: 2017-08-28 23:03:24
tags: LeetCode
categories: LeetCode
---

Given an array of n positive integers and a positive integer s, find the minimal length of a contiguous subarray of which the sum ≥ s. If there isn't one, return 0 instead.

For example, given the array [2,3,1,2,4,3] and s = 7,
the subarray [4,3] has the minimal length under the problem constraint.
<!--more-->

## 题意：

　　给定含有n个正整数的数组和一个正整数，求最小长度的子数组，使子数组的和sum ≥ s，如果不存在这样的子数组，返回0。

## 思路：

### 　方法一：

　　利用滑动窗口机制，用两个指针来维持和的最大值，然后求最小子数组长度。pre和last指针之间维持着大于s的和的子数组，通过减小和扩大滑动窗口大小，来实现查找对小的和大于s的子数组，时间复杂度是O(n)。

```c++
class Solution {
public:
	int minSubArrayLen(int s, vector<int>& nums) {
		int res = 0;
		int len = nums.size();
		if (len==0)
		{
			return res;
		}
		int pre = 0, last = 0;
		int minLen = INT_MAX;
		int sum = nums[pre];
		while (pre<len)
		{
			if (sum<s)
			{
				if (pre == len-1)
				{
					break;
				}
				pre++;//移动滑动窗口前指针
				sum += nums[pre];
			}
			else
			{
				int dis = pre - last + 1;
				minLen = min(minLen, dis);
				sum -= nums[last];
				last++;//移动滑动窗口后指针
			}
		}
		return minLen==INT_MAX?res:minLen;
	}
};
```

### 　方法二：

　　时间复杂度是O(nlogn)的算法。

```
Now let's move on to the O(nlogn) solution. Well, this less efficient solution is far more difficult to come up with. The idea is to first maintain an array of accumulated summations of elements in nums. Specifically, for nums = [2, 3, 1, 2, 4, 3] in the problem statement, sums = [0, 2, 5, 6, 8, 12, 15]. Then for each element in sums, if it is not less than s, we search for the first element that is greater than sums[i] - s (in fact, this is just what the upper_bound function does) in sums using binary search.

Let's do an example. Suppose we reach 12 in sums, which is greater than s = 7. We then search for the first element in sums that is greater than sums[i] - s = 12 - 7 = 5 and we find 6. Then we know that the elements in nums that correspond to 6, 8, 12 sum to a number 12 - 5 = 7 which is not less than s = 7. Let's check for that: 6 in sums corresponds to 1 in nums, 8 in sums corresponds to 2 in nums, 12 in sums corresponds to 4 in nums. 1, 2, 4 sum to 7, which is 12 in sums minus 5 in sums.

We add a 0 in the first position of sums to account for cases like nums = [3], s = 3.
```

```c++
class Solution {
public:
	int minSubArrayLen(int s, vector<int>& nums) {
		int res = 0;
		int len = nums.size();
		if (len == 0)
		{
			return res;
		}
		int minLen = INT_MAX;
		vector<int> sumNum(len + 1, 0);
		for (int i = 0; i < len; i++)
		{
			sumNum[i + 1] = sumNum[i] + nums[i];
		}
		for (int i = 0; i < len; i++)
		{
			int l = i + 1;
			int r = len;
			int target = sumNum[i] + s;
			if (target > sumNum[r])
				break;
			if (target < sumNum[l])
				return 1;
			while (l < r)
			{
				int mid = (l + r) / 2;
				if (sumNum[mid] < target)
					l = mid + 1;//特别注意此处，当sumNum[mid] == target的时候坚决不能让mid+1，否则会造成元素多1个，只要大于等于target的时候一定躺r=mid,不要移动l
				else
					r = mid;
			}
			minLen = min(minLen, l - i);
		}
		return minLen == INT_MAX ? res : minLen;
	}
};
```



