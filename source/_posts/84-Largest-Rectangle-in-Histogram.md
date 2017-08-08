---
title: 84. Largest Rectangle in Histogram
date: 2017-08-08 22:52:12
tags: LeetCode
categories: LeetCode
---

# 84. Largest Rectangle in Histogram

Given n non - negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

![84-1-histogram](/images/84-1-histogram.png)

Above is a histogram where width of each bar is 1, given height = [2, 1, 5, 6, 2, 3].
![84-2-histogram_area](/images/84-2-histogram_area.png)
The largest rectangle is shown in the shaded area, which has area = 10 unit.

For example,
Given heights = [2, 1, 5, 6, 2, 3],
return 10.

<!--more-->

## 题意：

　　给定n个非负整数表示直方图的条形高度，每个条的宽度为1，找到直方图中最大矩形的面积。

## 思路：

### 　方法一：

　　1、如果已知height数组是升序
　　　　比如1, 2, 5, 7, 8
　　　　那么就是(1 * 5) vs. (2 * 4) vs. (5 * 3) vs. (7 * 2) vs. (8 * 1)
　　　　也就是max(height[i] * (size - i))
　　2、使用栈的目的就是构造这样的升序序列，按照以上方法求解。
　　　　但是height本身不一定是升序的，应该怎样构建栈？
　　　　比如2, 1, 5, 6, 2, 3
　　　　　　（1）2进栈。s = { 2 }, result = 0
　　　　　　（2）1比2小，不满足升序条件，因此将2弹出，并记录当前结果为2 * 1 = 2。
　　　　　　　　　将2替换为1重新进栈。s = { 1,1 }, result = 2
　　　　　　（3）5比1大，满足升序条件，进栈。s = { 1,1,5 }, result = 2
　　　　　　（4）6比5大，满足升序条件，进栈。s = { 1,1,5,6 }, result = 2
　　　　　　（5）2比6小，不满足升序条件，因此将6弹出，并记录当前结果为6 * 1 = 6。s = { 1,1,5 }, result = 6
　　　　　　　　2比5小，不满足升序条件，因此将5弹出，并记录当前结果为5 * 2 = 10（因为已经弹出的5, 6是升序的）。s = { 1,1 }, result = 10
　　　　　　　　2比1大，将弹出的5, 6替换为2重新进栈。s = { 1,1,2,2,2 }, result = 10
　　　　　　（6）3比2大，满足升序条件，进栈。s = { 1,1,2,2,2,3 }, result = 10
　　　　栈构建完成，满足升序条件，因此按照升序处理办法得到上述的max(height[i] * (size - i)) = max{ 3 * 1, 2 * 2, 2 * 3, 2 * 4, 1 * 5, 1 * 6 } = 8 < 10
　　　　综上所述，result = 10

```c++
int largestRectangleArea(vector<int>& heights)
{
	int res = 0;
	stack<int> s;
	for (auto val : heights)
	{
		if (s.empty() || s.top()<= val)
		{
			s.push(val);
		}
		else {
			int count = 0;
			while (!s.empty()&&s.top()>val)//通过栈构造递增序列，然后求最大面积
			{
				count++;
				res = max(res, s.top()*count);
				s.pop();
			}
			while (count)
			{
				s.push(val);
				count--;
			}
			s.push(val);
		}
	}
	int dis = 1;
	while (!s.empty())
	{
		res = max(res,s.top()*dis++);
		s.pop();
	}
	return res;
}
```

### 　方法二：

　　思路同方法一，也是通过栈来狗仔递增柱状图序列，但是此法的巧妙之处不是用栈来存在柱状图数组中的值，而是存储柱状图数组的索引。
　　（1）当遍历的当前元素比栈顶元素大或者相等的时候，直接把其索引入栈
　　（2）当遍历的当前元素比栈顶元素小的时候，弹出栈顶索引，计算弹出的索引（其实是栈顶索引，记录弹出索引是为了去元素值，栈顶元素索引 - 当前遍历索引 - 1）和当前遍历到的那个小的索引差值，然后减一，刚刚好得到所有比弹出元素大，而且比当前遍历元素大的元素个数，然后计算出面积值

```c++
int largestRectangleArea(vector<int> &height) 
{
	height.push_back(0);
	stack<int> s;
	int maxRrea=0;
	int i = 0;
	while (i<height.size())
	{
		if (s.empty() || height[s.top()] <= height[i])
		{
			s.push(i++);//放入当前元素索引
		}
		else
		{
			int tempArea = 0;
			int index = s.top();//因为当前元素比栈顶索引所对应元素小，所以弹出栈顶索引
			s.pop();
			if (s.empty())//弹出栈顶要判断是否为空，如果为空，说明index所指向元素比以前元素都小，所以可以直接height[index] * i，i表示所有height[index]高度可以组成面积的柱方图个数
			{
				tempArea = height[index] * i;
				maxRrea = maxRrea > tempArea ? maxRrea : tempArea;
			}
			else//不为空的话就要通过现在栈顶元素索引和i来求height[index]高度可以组成面积的柱方图个数
			{
				tempArea = height[index] *(i - s.top() - 1);
				maxRrea = maxRrea > tempArea ? maxRrea : tempArea;
			}
		}
	}
	return maxRrea;
}
```

