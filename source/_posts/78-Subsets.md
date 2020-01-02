---
title: 78. Subsets
date: 2017-08-06 22:40:33
tags: LeetCode
categories: LeetCode
---

# 78. Subsets

Given a set of distinct integers, nums, return all possible subsets.

Note: The solution set must not contain duplicate subsets.

For example,
If nums = [1,2,3], a solution is:
[
　　[3],
　　[1],
　　[2],
　　[1,2,3],
　　[1,3],
　　[2,3],
　　[1,2],
　　[]
]

<!--more-->

## 题意：

　　给出一组不同的整数集合，返回所有可能的子集。

　　注意：结果集不能包含重复子集。

## 思路：

### 　方法一：

　　穷举不同元素个数的所有子集集合，符合递归回溯思想，穷举所有结果，控制递归结束条件为子集元素个数。

```c++
class Solution {
public:
	vector<vector<int>> res;
	vector<vector<int>> subsets(vector<int>& nums) {
		vector<int> tempRes;
		int sizes = nums.size();
		for (int step = 0;step<=sizes;step++)
		{
			backtracking(nums,tempRes, step, sizes,0);
		}
		return res;
	}
	void backtracking(vector<int>& nums,vector<int> tempRes, int k, int sizes,int index) {
		if (k==0)
		{
			res.push_back(tempRes);
			return;
		}
		for (int i = index;i<=sizes-k;i++)//i = index表明不能有重复元素，直接取集合的下一个元素
		{
			tempRes.push_back(nums[i]);
			backtracking(nums, tempRes, k - 1, sizes, i + 1);//每放进去一个元素，子集元素个数减一，即k-1
			tempRes.pop_back();
		}
	}
};
```

### 　方法二：

　　循环迭代穷举不同元素个数的所有子集集合。

```c++
class Solution {
public:
	vector<vector<int>> subsets(vector<int>& nums) {
		vector<vector<int>> res(1,vector<int>());
		sort(nums.begin(), nums.end());//先对集合进行升序排序
		int sizes = nums.size();
		for (int i = 0; i < sizes; i++)
		{
			int n = res.size();
			for (int j = 0;j<n;j++)
			{
				res.push_back(res[j]);
				res.back().push_back(nums[i]);
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
    public List<List<Integer>> subsets(int[] nums) {
       List<List<Integer>> res = new ArrayList();
       if (null == nums) {
           return res;
       }

       int n = nums.length;
       if (n == 0) {
           return res;
       }

       for (int i = 0;i <= n;i++) {
           List<Integer> subList = new ArrayList();
           List<List<Integer>> tmpRes = new ArrayList();
           getSubSet(nums, 0, i, subList, tmpRes);
           res.addAll(tmpRes);
       }

       return res;
    }

    public void getSubSet(int[] nums, int index, int subSize, List<Integer> subList, List<List<Integer>> tmpRes) {
        if (subSize == 0) {
            tmpRes.add(subList);
            return;
        }

        for (int i = index;i < (nums.length - subSize + 1);i++) {
            List<Integer> tmpSubList = new ArrayList(subList);
            tmpSubList.add(nums[i]);
            getSubSet(nums, i + 1, subSize - 1, tmpSubList, tmpRes);
        }
    }
}
```
