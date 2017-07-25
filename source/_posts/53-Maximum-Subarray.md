---
title: 53. Maximum Subarray
date: 2017-07-25 21:01:05
tags: LeetCode
categories: LeetCode
---

# 53. Maximum Subarray

Find the contiguous subarray within an array (containing at least one number) which has the largest sum.

For example, given the array [-2,1,-3,4,-1,2,1,-5,4],
the contiguous subarray [4,-1,2,1] has the largest sum = 6.

More practice:
If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.

<!--more-->

## 题意：

　　在数组中找到连续子数组（子数组至少包含一个数字），使连续子数组的和最大。
　　如果您已经解决了O（n）的解决方案，请尝试使用分而治之的方法编写另一个解决方案，这种解法更好。

## 思路：

　　保留一个求和值，一个结果值，求和值小于零时直接置为0，结果值不断和求和值不断比较求出最大值。

```c++
class Solution {
public:
	//For example, given the array [-2,1,-3,4,-1,2,1,-5,4],
	//the contiguous subarray [4,-1,2,1] has the largest sum = 6.
	int maxSubArray(vector<int>& nums) {
		int res = INT_MIN;
		int len = nums.size();
		if (len == 0)
		{
			return  0;
		}
		if (len == 1)
		{
			return nums[0];
		}
		int maxVal = 0;
		for (int i = 0; i < len; i++)
		{
			maxVal += nums[i];
			res = max(maxVal, res);
			if (maxVal < 0)
			{
				maxVal = 0;
			}
		}
		return res;
	}
};
```

