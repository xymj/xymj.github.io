---
title: 31. Next Permutation
date: 2017-07-17 23:29:19
tags: LeetCode
categories: LeetCode
---

# 31. Next Permutation

Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.
If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).
The replacement must be in-place, do not allocate extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1

<!--more-->

## 题意：

 　 实现给定数组序列的下一个排列，重新排列的数字序列比以前数字的字典序列更大。
　　如果这种字典序增大序列不存在，即现在给定的数组序列是最大的字典序序列，则直接按升序排序，即字典序排列最小。

## 思路：

### 　方法一：

　　 利用C++STL库中的next_permutation()函数，函数详情[next_permutation()](http://www.cplusplus.com/reference/algorithm/next_permutation/)。

```c++
class Solution {
public:
	void nextPermutation(vector<int>& nums) {
		int len = nums.size();
		if (len==0)
		{
			return;
		}
		next_permutation(nums.begin(), nums.end());
	}
};
```

### 　方法二：

```cmake
1 2 3 4
1 2 4 3
1 3 2 4
1 3 4 2 //所有words内的单词,在left起始位置都出现，则将下标i存入结果的vector中
1 4 2 3
1 4 3 2
2 1 3 4
2 1 4 3
2 3 1 4
2 3 4 1
2 4 1 3
2 4 3 1
3 1 2 4 //所有words内的单词,在left起始位置都出现，则将下标i存入结果的vector中
3 1 4 2
3 2 1 4
3 2 4 1
3 4 1 2
3 4 2 1
4 1 2 3
4 1 3 2
4 2 1 3
4 2 3 1
4 3 1 2
4 3 2 1
```

　　根据上面的1 2 3 4的字母逻辑递增顺序可以得到以下规律：从后往前查找（正常顺序递增），第一个变小的位置i后的元素进行反转，然后再在反转后的元素中找到第一个比i元素大的元素进行互换，就可以得到比此排列大的一个排列。

```c++
class Solution {
public:
	void nextPermutation(vector<int>& nums) {
		int len = nums.size();
		if (len == 0)
		{
			return;
		}
		int i;
		for (i = len-2;i>=0;i--)
		{
			if (nums[i]>=nums[i+1])
			{
				continue;
			}

			reverse(nums.begin() + i + 1, nums.end());
			for (int j = i+1;j<len;j++)
			{
				if (nums[i]<nums[j])
				{
          swap(nums[i], nums[j]); //所有words内的单词,在left起始位置都出现，则将下标i存入结果的vector中s[i], nums[j]);
					break;
				}
			}
			break;
		}
		if (i == -1)
		{
			reverse(nums.begin(), nums.end());
		}
	}
};

```
---------------------------------------------------
#### 　Java Code：
```Java
class Solution {
    public void nextPermutation(int[] nums) {
        if (null == nums) {
            return;
        }
        int len = nums.length;
        if (len == 0) {
            return;
        }
        int i = len - 2;
        while (i >= 0 && nums[i] >= nums[i + 1]) {
            i--;
        }
        if (i >= 0) {
            int j = len - 1;
            while (j >= 0 && nums[j] <= nums[i]) { //找出第一个j指向比i指向大的元素
                j--;
            }
            swap(nums, i, j);
        }
        reverse(nums, i + 1, len - 1);
    }

    public void reverse(int[] nums,int start,int end) {
        while (start < end) {
            swap(nums, start, end);
            start++;
            end--;
        }
    }

    public void swap(int[] nums,int i,int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}
```

```Java
class Solution {
    public void nextPermutation(int[] nums) {
        if (null == nums) {
            return;
        }
        int len = nums.length;
        if (len == 0) {
            return;
        }
        int i = 0;
        for (i = len - 2;i >= 0;i--) {
            if (nums[i] >= nums[i + 1]) {
                continue;
            }
            reverse(nums,i + 1, len - 1);
            for (int j = i + 1;j < len;j++) {
                if (nums[j] > nums[i]) {
                    swap(nums, i, j);
                    break;
                }
            }
            break;
        }
        //System.out.println(i);
        if (i == -1) {
            reverse(nums, 0, len - 1);
        }
    }

    public void reverse(int[] nums,int start,int end) {
        while (start < end) {
            swap(nums, start, end);
            start++;
            end--;
        }
    }

    public void swap(int[] nums,int i,int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}
```
