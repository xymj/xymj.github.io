---
title: 56. Merge Intervals
date: 2017-07-25 21:43:42
tags: LeetCode
categories: LeetCode
---

# 56. Merge Intervals

Given a collection of intervals, merge all overlapping intervals.

For example,
Given[1, 3], [2, 6], [8, 10], [15, 18],
return[1, 6], [8, 10], [15, 18].

<!--more-->

## 题意：

　　给定一个间隔集合，合并所有重叠的区间间隔。

## 思路：

　　（1）先将目标区间数组按X轴从小到大排序。例如：\[2 ,3]\[1, 2]\[3, 9] ->\[1, 2]\[2, 3]\[3, 9]
　　（2）扫描排序后的目标区间数组，将这些区间合并成若干个互不相交的区间。例如\[2, 3]\[1, 2]\[4, 9] ->\[1, 3]\[4, 9]
　　这里分三种情况：
　　　　a :\[1, 3]\[2, 6] ->\[1, 6] 第一个区间的end大于等于第二个区间的start, 同时第二个区间的end大于第一个区间的end
　　　　b :\[1, 7]\[2, 4] ->\[1, 7] 第一个区间的end大于等于第二个区间的start, 同时第二个区间的end小于第一个区间的end
　　　　c :\[1, 2]\[3, 4] ->\[1, 2][3, 4] 第一个区间的end小于第二个区间的start

```c++
/**
* Definition for an interval.
*/
struct Interval {
     int start;
     int end;
     Interval() : start(0), end(0) {}
     Interval(int s, int e) : start(s), end(e) {}
};

class Solution{
   vector<Interval> merge(vector<Interval>& intervals) {
        vector<Interval> result;
        if (intervals.size() <= 1)
        {
            return intervals;
        }
        sort(intervals.begin(), intervals.end(), [](Interval a, Interval b) {return a.start < b.start; });
        Interval node = intervals[0];
        for (int i=1;i<intervals.size();i++)
        {
            Interval tempNode = intervals[i];
            if (node.end<tempNode.start)
            {
                result.push_back(node);
                node = tempNode;
                continue;
            }
            else
            {
                node.end = max(node.end, tempNode.end);
            }
        }
        result.push_back(node);//用于控制最后一个节点区间
        return result;
    }
};
```

---------------------------------------------------
### 　Java Code
```Java
class Solution {

    public int[][] merge(int[][] intervals) {
        if (null == intervals) {
            return null;
        }

        int row = intervals.length;
        if (row == 0) {
            return new int[0][0];
        }

        int col = intervals[0].length;

        List<Interval> vals = new ArrayList();
        for (int i = 0;i < row;i++) {
            vals.add(new Interval(intervals[i][0], intervals[i][1]));
        }

        //Comparator.comparing()
        List<Interval> sortVals = vals.stream()
            .sorted(Comparator.comparingInt(Interval::getHead))
            .collect(Collectors.toList());
        List<Interval> resVals = new ArrayList();
        Interval firstVal = sortVals.get(0);
        for (Interval val : sortVals) {
            if (firstVal.tail >= val.head) {
                firstVal.tail = Math.max(firstVal.tail, val.tail);
            } else {
                resVals.add(firstVal);
                firstVal = val;
            }
        }
        resVals.add(firstVal);

        int resSize = resVals.size();
        int[][] res = new int[resSize][2];
        for (int i = 0;i < resSize;i++) {
            res[i][0] = resVals.get(i).head;
            res[i][1] = resVals.get(i).tail;
        }

        return res;
    }

    class Interval {
        public int head;
        public int tail;

        public Interval(int head, int tail) {
            this.head = head;
            this.tail = tail;
        }

        public int getHead() {
            return head;
        }
    }
}
```
