---
title: 219. Contains Duplicate II
date: 2017-08-30 23:18:27
tags: LeetCode
categories: LeetCode
---

Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array 
such that nums[i] = nums[j] and the difference between i and j is at most k.

<!--more-->

## 题意：

　　给定整数数组和整数k，找出数组中是否有两个不同的索引i和j，使得nums [i] = nums [j]，i和j之间的差最多为k。

## 思路：

　　利用hash表，key为元素值，value为此元素所对应的索引的集合，遍历元素组，把相同元素的索引都放入到对应hash表的value集合中，然后遍历hash表，找出集合大小大于1的value集合，遍历此集合，求出相同元素索引间最小距离。

```c++
class Solution {
public:
	bool containsNearbyDuplicate(vector<int>& nums, int k) {
		if (nums.empty())
		{
			return false;
		}
		map<int, vector<int>> hash;
		for (int i = 0; i < nums.size(); i++)
		{
			hash[nums[i]].push_back(i);
		}
		for (auto itr = hash.begin();itr!=hash.end();itr++)
		{
			if (itr->second.size()>1)
			{
				int minIndex = INT_MAX;
				for (int j = 1; j < itr->second.size(); j++)
				{
					int m = j - 1;
					minIndex = min(minIndex, itr->second[j] - itr->second[m]);
				}
				if (minIndex<=k)
				{
					return true;
				}
			}
		}
		return false;
	}
};
```

### 　方法二：

　　思路同方法一，利用hash表，不同点是一边遍历数组，一边在hash表对应的索引值进行相减比较。

```c++
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        if (nums.empty())
        {
            return false;
        }
        map<int, int> hash;
        for (int i = 0; i < nums.size(); i++)
        {
            auto itr = hash.find(nums[i]);
            if (itr != hash.end())
            {
                if (i - itr->second <= k)
                {
                    return true;
                }
                else
                    hash.erase(itr);
            }
            hash[nums[i]] = i;
        }
        return false;
    }
};
```

