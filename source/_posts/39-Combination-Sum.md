---
title: 39. Combination Sum
date: 2017-07-19 22:46:23
tags: LeetCode
categories: LeetCode
---

# 39. Combination Sum

Given a set of candidate numbers (C) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.
The same repeated number may be chosen from C unlimited number of times.

Note:
　　All numbers (including target) will be positive integers.
　　The solution set must not contain duplicate combinations.

For example, given candidate set [2, 3, 6, 7] and target 7,
A solution set is:
[
　[7],
　[2, 2, 3]
]

<!---more-->

## 题意：

　　给定一组候选数字（C），数组中没有重复元素，和一个目标数（T），在C中找出所有唯一的组合，其中候选数总和为T。
同样的重复数可以从C无限次数选择。

## 思路：

　　主要是通过递归回溯来进行元素组合的查找，每次递归下一层，判断所取元素的和与目标值是否相等，相等则是一个结果，因为一个元素可以重复取，所以每次递归下层，数组的索引不用前进1，而且在递归回溯开始前先对数组进行排序，便于后面条件判断的剪枝，减少递归层数，提高效率。

```c++
class Solution {
public:
	vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
		vector<vector<int>> res;
		vector<int> sumNum;//记录组合元素
		sort(candidates.begin(), candidates.end());//先排序，便于剪枝，减少递归层数
		backtracking(candidates, target, 0, 0, sumNum, res);
		return res;
	}
	void backtracking(vector<int> candidates, int target, int index, int sum, vector<int> sumNum, vector<vector<int>> &res) {
		if (sum == target)
		{
			res.push_back(sumNum);
			return;
		}
		else
		{
			for (int i = index; i < candidates.size(); i++)
			{
				if (sum + candidates[i]>target)//已经排序，可以剪枝
				{
					break;
				}
				sumNum.push_back(candidates[i]);
				backtracking(candidates, target, i, sum + candidates[i], sumNum, res);
				sumNum.pop_back();
			}
		}
	}
};
```

---------------------------------------------------
#### 　Java Code：
```Java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        if (null == candidates) {
            return new ArrayList();
        }

        int len = candidates.length;
        if (0 == len) {
            return new ArrayList();
        }

        Arrays.sort(candidates);
        List<Integer> sumElements = new ArrayList();
        List<List<Integer>> res = new ArrayList();
        getCombinations(candidates, 0, 0, target, sumElements, res);
        return res;
    }

    public void getCombinations(int[] candidates, int index, int sum, int target,
                                List<Integer> sumElements, List<List<Integer>> res) {
        if (sum == target) {
            res.add(sumElements);
            return;
        }

        for (int i = index; i < candidates.length; i++) {
            if (sum + candidates[i] > target) {
                break;
            }
            List<Integer> tmpSumElements = new ArrayList(sumElements);//直接在进入下一层的时候复制结果集，减少退出下一层递归时删除元素操作，而且往结果集里面添加的也是创建的结果组合副本
            tmpSumElements.add(candidates[i]);
            getCombinations(candidates, i, sum + candidates[i], target, tmpSumElements, res);
        }
    }
}
```

```Java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        if (null == candidates) {
            return new ArrayList();
        }

        int len = candidates.length;
        if (0 == len) {
            return new ArrayList();
        }

        Arrays.sort(candidates);
        List<Integer> sumElements = new ArrayList();
        List<List<Integer>> res = new ArrayList();
        getCombinations(candidates, 0, 0, target, sumElements, res);
        return res;
    }

    public void getCombinations(int[] candidates, int index, int sum, int target,
                                List<Integer> sumElements, List<List<Integer>> res) {
        if (sum == target) {
            res.add(sumElements.stream().collect(Collectors.toList()));//res内存的是sumElements的地址，后面删除会影响结果集，所以要从新构建一个新的组合结果放到结果集里面
            return;
        }

        for (int i = index; i < candidates.length; i++) {
            if (sum + candidates[i] > target) {
                break;
            }
            sumElements.add(candidates[i]);
            getCombinations(candidates, i, sum + candidates[i], target, sumElements, res);
            sumElements.remove(Integer.valueOf(candidates[i]));//通过对象删除
        }
    }
}
```
