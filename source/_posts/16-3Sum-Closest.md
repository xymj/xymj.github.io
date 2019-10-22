---
title: 16. 3Sum Closest
date: 2017-07-03 22:00:51
tags: LeetCode
categories: LeetCode
---

# 16. 3Sum Closest

Given an array S of n integers, find three integers in S such that the sum is closest to a given number, target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

For example, given array S = {-1 2 1 -4}, and target = 1.
The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).

<!--more-->

## 题意：

　　给一个含有n个整数的S数组，查找在S中的三元组，使三个数字的和最靠近目标值 ，返回最接近目标值的三个整数的和。假设每个输入都已一个确切的答案。

## 思路：

　　思想大致都和15. 3Sum相同，先排序，然后利用三个指针，一个指针指向固定元素，另外两个做为这个固定元素后面的两个移动指针，然后三元素求和，当和大于目标值的时候说明所求和大，需减小，所以把尾指针前移，小于目标值的时候说明所求和小，需增大，所以把首指针后移

```c++
class Solution {
public:
	int threeSumClosest(vector<int>& nums, int target) {
		int len = nums.size();
		sort(nums.begin(), nums.end());
		int sum = 0, dis = INT_MAX;
		for (int i = 0;i<len-2;i++)
		{
			int pre = i + 1;
			int tail = len - 1;
			while (pre<tail)
			{
				int tempSum = nums[i] + nums[pre] + nums[tail];
				int tempDis = abs(target - tempSum);
				if (tempDis < dis)
				{
					sum = tempSum;
					dis = tempDis;
				}
				if (tempSum < target)
				{
					pre++;
					while (pre < tail&&nums[pre] == nums[pre - 1])
					{
						pre++;//去除重复元素，类似15. 3Sum
					}
				}
				else
				{
					tail--;
					while (pre < tail&&nums[tail] == nums[tail + 1])
					{
						tail--;//去除重复元素，类似15. 3Sum
					}
				}

			}
		}
		return sum;
	}
};
```

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        if (null == nums) {
            return 0;
        }
        int len = nums.length;
        if (len == 0) {
            return 0;
        }
        Arrays.sort(nums);
        int minDis = Integer.MAX_VALUE;
        int res = 0;

        for (int i = 0;i < len - 2;i++) {
            int curNum = nums[i];

            int pre = i + 1;
            int tail = len - 1;
            while (pre < tail) {
                int preNum = nums[pre];
                int tailNum = nums[tail];

                int sum = curNum + preNum + tailNum;
                int dis = Math.abs(target - sum);

                if (dis < minDis) {
                    res = sum;
                    minDis = dis;
                }

                if (target > sum) {
                    pre++;
                    while (pre < tail && nums[pre -1] == nums[pre]) {
                        pre++;
                    }
                } else {
                    tail--;
                    while (pre < tail && nums[tail + 1] == nums[tail]) {
                        tail--;
                    }
                }
            }
        }

        return res;
    }
}
```
