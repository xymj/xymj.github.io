---
title: 27. Remove Element
date: 2017-07-12 22:09:11
tags: LeetCode
categories: LeetCode
---

# 27. Remove Element

Given an array and a value, remove all instances of that value in place and return the new length.
Do not allocate extra space for another array, you must do this in place with constant memory.
The order of elements can be changed. It doesn't matter what you leave beyond the new length.

Example:
Given input array nums = [3,2,2,3], val = 3
Your function should return length = 2, with the first two elements of nums being 2.

<!--more-->

## 题意：

　　给定一个数组和一个值，删除该值的所有实例并返回数组新长度。不要分配使用额外的数组空间，必须在常量内存空间中执行此操作。元素的顺序可以改变。**特别注意**不用考虑超过你返回长度的数组元素值，也就是你返回长度是多少，数组元素就有多少个，且不包含指定的元素值。

## 思路：

### 　方法一：

　　利用数组迭代器，直接删除元素，把和目标值相等的元素直接删除。但是此方法效率低，不太符合题意。

```C++
//You are here! Your runtime beats 19.53% of cpp submissions.
//113 / 113 test cases passed.
//Status: Accepted
//Runtime: 3 ms
class Solution{
public:
	int removeElement(vector<int>& nums, int val) {
		if (nums.size() == 0)
		{
			return nums.size();
		}
		vector<int>::iterator itr = nums.begin()+1;
		vector<int>::iterator tempItr = nums.begin();
		while (itr != nums.end())
		{
			if (*itr == val)
			{
				nums.erase(itr);
				itr = tempItr;
				itr++;
			}
			else
			{
				itr++;
				tempItr++;
			}
		}
		if (nums[0]==val)
		{
			nums.erase(nums.begin());
		}
		return nums.size();
	}
};
```

### 　方法二：

　　不一定非得把找到的元素删除掉，把要找到的元素覆盖掉就行，并不真正改变原数组的大小，这个题的关键就是根据你返回的不是要查找元素的个数cnt，那么编译器就自己取数组的前cnt个元素，所以省去了自己截取前cnt个元素为新数组的步骤。

```c++
class Solution {
public:
	int removeElement(vector<int>& nums, int val) {
		int cnt = 0;
		for (int i = 0;i<nums.size();i++)
		{
			if (nums[i]!=val)
			{
				nums[cnt] = nums[i];//把相等的目标值元素覆盖掉
				cnt++;
			}
		}
		return cnt;//直接返回除去目标元素后剩下元素的个数
	}
};
```

---------------------------------------------------
#### 　Java Code：
```java
class Solution {
    public int removeElement(int[] nums, int val) {
        if (null == nums) {
            return 0;
        }
        int len = nums.length;
        int count = 0;
        for (int i = 0;i < len;i++) {
            if (val != nums[i]) {
                nums[count] = nums[i];
                count++;
            }
        }
        return count;
    }
}
```
