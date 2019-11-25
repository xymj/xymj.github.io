---
title: 47. Permutations II
date: 2017-07-21 21:49:58
tags: LeetCode
categories: LeetCode
---

# 47. Permutations II

Given a collection of numbers that might contain duplicates, return all possible unique permutations.

For example,
[1,1,2] have the following unique permutations:
[
　　[1,1,2],
　　[1,2,1],
　　[2,1,1]
]

<!--more-->

## 题意：

### 　方法一：

　　意思同46. Permutations，不同的就是给定的数组集合是有重复元素出现的，但是结果集中不能出现重复的结果排列。

## 思路：

　　递归回溯法，先把给定集合排序，然后在递归回溯的过程中，当遍历同一层的元素的时候要比较元素是否相等，相等要进行剪枝，因为同一层相同元素出现并入结果集中肯定会出现重复结果组合。

```c++
class Solution {
public:
	vector<vector<int>> res;
	vector<vector<int>> permuteUnique(vector<int>& nums) {
		sort(nums.begin(), nums.end());//先进行排序，便于剪枝
		int sizes = nums.size();
		backtracting(nums, 0, sizes);
		return res;
	}
	void backtracting(vector<int> nums, int index, int sizes) {
		if (index==sizes)
		{
			res.push_back(nums);
			return;
		}
		for (int i = index; i < sizes; i++) {
			if (i != index&&nums[i] == nums[index])//同层出现相同元素，直接跳过遍历下一个元素，i!=index说明i和index所指向的元素位于同一层
			{
				continue;
			}
			swap(nums[i], nums[index]);
			backtracting(nums, index + 1, sizes);
		}
	}
};
```

### 　方法二：

　　直接利用C++的STL函数库next_permutation()函数，函数详情[next_permutation()](http://www.cplusplus.com/reference/algorithm/next_permutation/) 。

```c++
class Solution {
public:
	vector<vector<int>> permuteUnique(vector<int>& nums) {
		vector<vector<int>> res;
		sort(nums.begin(), nums.end());
		res.push_back(nums);
		while (next_permutation(nums.begin(),nums.end()))
		{
			res.push_back(nums);
		}
		return res;
	}
};
```


---------------------------------------------------
#### 　Java Code：
```Java
class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        if (null == nums) {
            return new ArrayList();
        }

        int len = nums.length;
        if (len == 0) {
            return new ArrayList();
        }

        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList();
        List<Integer> elements = new ArrayList();
        int[] flag = new int[len];
        nextPermutations(nums, flag, elements, res);

        return res;
    }

    public void nextPermutations(int[] nums, int[] flag, List<Integer> elements, List<List<Integer>> res) {
        if (elements.size() == nums.length) {
            res.add(elements);
            return;
        }

        for (int i = 0;i < nums.length;i++) {
            if (flag[i] == 0) {
                if (i > 0 && nums[i] == nums[i - 1] && flag[i - 1] == 1) {
                    continue;
                }
                flag[i] = 1;
                List<Integer> tmpElements = new ArrayList(elements);
                tmpElements.add(nums[i]);
                nextPermutations(nums, flag, tmpElements, res);
                flag[i] = 0;
            }
        }
    }
}
```
