---
title: 57. Insert Interval
date: 2017-07-25 23:24:54
tags: LeetCode
categories: LeetCode
---

# 57. Insert Interval

Given a set of non - overlapping intervals, insert a new interval into the intervals(merge if necessary).
You may assume that the intervals were initially sorted according to their start times.

Example 1:
Given intervals[1, 3], [6, 9], insert and merge[2, 5] in as[1, 5], [6, 9].

Example 2 :
​	Given[1, 2], [3, 5], [6, 7], [8, 10], [12, 16], insert and merge[4, 9] in as[1, 2], [3, 10], [12, 16].
​	This is because the new interval[4, 9] overlaps with[3, 5], [6, 7], [8, 10].

<!--more-->

## 题意：

　　给定一组不重叠的区间，在给定区间中插入一个新的区间（必要时合并）。假设区间的根据它们的开始时间排序的。**注意：**区间初始按开始时间排过序。

## 思路：

　　这道题的要求是给定一组非重叠且开始时间有序的间隔，在其中插入一个新的间隔。这道题可以借助[56. Merge Intervals](http://blog.taoaili999.cn/2017/07/25/56-Merge-Intervals/)里面的代码，先将新的间隔加入到数组中，然后合并即可。时间复杂度是O(nlogn)。
　　事实上，也可以不排序，直接插入时间间隔，插入的时间间隔的位置可以分成三部分：
　　　　a 插入位置左侧
　　　　b 插入位置（有重叠或无重叠）
　　　　c 插入位置右侧
　　这三个部分分别处理，只有在插入位置处理可能存在的情况即可。
　　时间复杂度：O(n)
　　空间复杂度：O(n)

```c++
vector<Interval> insert(vector<Interval>& intervals, Interval newInterval) {
	if (intervals.empty())
	{
		intervals.push_back(newInterval);
		return intervals;
	}
	vector<Interval> result;
	int i = 0;
	while (i < intervals.size()&&intervals[i].end<newInterval.start)
	{
		result.push_back(intervals[i]);
		i++;
	}
	result.push_back(newInterval);
	while (i<intervals.size()&&result[result.size()-1].end>=intervals[i].start)
	{
		result[result.size() - 1].start = min(result[result.size() - 1].start, intervals[i].start);
		result[result.size() - 1].end = max(result[result.size() - 1].end, intervals[i].end);
		i++;
	}
	while (i<intervals.size())
	{
		result.push_back(intervals[i]);
		i++;
	}
	return result;
}
```

