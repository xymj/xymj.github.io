---
title: 149. Max Points on a Line
date: 2017-08-24 09:36:46
tags: LeetCode
categories: LeetCode
---

Given n points on a 2D plane, find the maximum number of points that lie on the same straight line.

## 题意：

　　在二维平面上给定n个坐标点，找落在同一条直线上点的个数，并找出落在同一直线上点最多的个数。 

<!--more-->

## 思路：

　　 首先注意：

```c++
  1）如果点的个数小于3个，则最大数目为点的个数；
　2）考虑重复点的情况，重复点是无法计算斜率的；
　3）考虑直线与y轴平行时，斜率为无穷大的情况。
```

　　 容易出错的地方：　　 　　 

```c++
　1）只考虑横坐标相等或纵坐标相等的情况，忽略斜率相等的情况，如{{0, 0}, {1, 1}, {-1, -1}}；
　2）忽略两点重合的情况，如{{1,, 1}, {1, 1}, {2, 2}, {2, 2}}；
  3）忽略只有一个点和两个点的情况；
```

　　 主要思路：选定一个点，分别计算其他点和它构成的直线的斜率，斜率相同的点肯定在同一条直线上。   计算斜率时，注意重合点和x值相同的两个点（数学上称斜率不存在，此时斜率用int的最大值表示）。

```c++
struct Point 
{
	int x;
	int y;
	Point() :x(0), y(0) {}
	Point(int a,int b) :x(a), y(b) {}

};
class Solution {
public:
	int maxPoints(vector<Point>& points) {
		if (points.size() <= 2)
		{
			return points.size();
		}
		int maxRes = 0;
		unordered_map<double, int> hash;
		for (int i = 0; i < points.size(); i++)//以每一个点为固定点
		{
			hash.clear();
			int duplicate = 1;//记录跟固定点重合的个数,初始化为1，代表固定点，避免最后计算个数的时候再加上一个固定点，因为hashmap中存储的都是除掉固定点以外的点。
			//j = i+1可以减少重复的检索判断，运行效率高 Your runtime beats 78.40% of cppsubmissions.
			//j = 0 会把以前遍历检索过的点重新检索判断一遍,效率低Your runtime beats 21.86% of cppsubmissions.
			for (int j = i+1; j < points.size(); j++)//遍历其它点，求其与固定点的斜率
			{
				//j=0为起始点开始循环的时候才需要判断i==j的自身情况
				if (i == j)//如果是点自身，跳过
				{
					continue;
				}
				else if((points[i].y == points[j].y)&&(points[i].x == points[j].x))//如果跟固定点重合
				{
					duplicate++;
					continue;
				}
				else
				{
					double k = 0.0;
					double m = 0.0;
					if (points[i].x == points[j].x)//如果垂直于x轴，设其斜率最大，即如果跟固定点在同一条竖线上，斜率设为最大
					{
						k = INT_MAX;
					}
					else {
						//此处要千万注意：首先要把分子转化为double类型，乘上1.0，不然整形和整形相除得到的还是整形会把斜率后面的小数忽略掉，然后整形再转换为double，这样精度就会不够，造成斜率只要整数部分相同就会断定在一条直线上出现错误。double/int 的结果值为double
					    k  = 1.0*(points[i].y - points[j].y) / (points[i].x - points[j].x);//计算斜率
					}
					if (hash.count(k))
					{
						hash[k]++;
					}
					else
					{
						hash[k] = 1;
					}
				}
			}
			if (hash.empty())
			{
				maxRes = duplicate > maxRes ? duplicate : maxRes;
			}
			else
			{
				for (auto h : hash)
				{
					if (h.second + duplicate > maxRes)
					{
						maxRes = h.second + duplicate;
					}
				}
			}
		}
		return maxRes;
	}
};
```

