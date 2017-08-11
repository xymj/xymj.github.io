---
title: 85. Maximal Rectangle
date: 2017-08-09 20:43:17
tags: LeetCode
categories: LeetCode
---

# 85. Maximal Rectangle

Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing all ones and return its area.

<!--more-->

## 题意：

　　给定一个二维二进制矩阵，填充0和1，找到由1围成的最大矩形，并返回它的面积。

## 思路：

###  方法一：

　　 基于[84-Largest-Rectangle-in-Histogram](http://blog.taoaili999.cn/2017/08/08/84-Largest-Rectangle-in-Histogram/)的思想，假设把矩阵沿着某一行分开，然后把分开的行作为底面，将自底面往上的矩阵看成一个直方图（histogram）。直方图的中每个项的高度就是从底面行开始往上1的数量。根据[84-Largest-Rectangle-in-Histogram](http://blog.taoaili999.cn/2017/08/08/84-Largest-Rectangle-in-Histogram/)就可以求出当前行作为矩阵下边缘的一个最大矩阵。接下来如果对每一行都做一次Largest Rectangle in Histogram，从其中选出最大的矩阵，那么它就是整个矩阵中面积最大的子矩阵。
　　如何计算某一行为底面时直方图的高度呢？如果重新计算，那么每次需要的计算数量就是当前行数乘以列数。然而会发现一些动态规划的踪迹，如果知道上一行直方图的高度，就只需要看新加进来的行（底面）上对应的列元素是不是0，如果是，则高度是0，否则则是上一行直方图的高度加1。利用历史信息，就可以在线行时间内完成对高度的更新。由于Largest Rectangle in Histogram的算法复杂度是O(n)。所以完成对一行为底边的矩阵求解复杂度是O(n+n)=O(n)。接下来对每一行都做一次，那么算法总时间复杂度是O(m*n)。
　　时间复杂度：O(mn)
　　空间复杂度：O(n)

```c++
class Solution {
public:
	int largestRectangleInHistogram(vector<int> &heights) {
		stack<int> s;
		int res = 0;
		//heights.push_back(0);
		if (heights.size()==0)
		{
			return res;
		}
		int i = 0;
		while (i < heights.size())
		{
			if (s.empty() || heights[s.top()]<=heights[i])
			{
				s.push(i++);
			}
			else
			{
				int t = s.top();
				s.pop();
				res = max(res, heights[t] * (s.empty() ? i : i - s.top() - 1));
			}
		}
		return res;
	}
	int maximalRectangle(vector<vector<char>>& matrix) {
		if (matrix.size()==0 || matrix[0].size()==0)
		{
			return 0;
		}
		int res = 0;
		vector<int> heights(matrix[0].size()+1, 0);//这个地方heights多加一列0，所以直方图计算中不用再添加数组末尾0，可以完成所有直方图列组成面积的计算
		for (int i = 0;i<matrix.size();i++)
		{
			for (int j = 0; j < matrix[i].size(); j++)
			{
				heights[j] = (matrix[i][j] == '0' ?  0 : heights[j] + 1);
			}
			res = max(res, largestRectangleInHistogram(heights));
		}
		return res;
	}
};
```

###  方法二：

　　 动态规划思想，思路同样是从第一行开始一行一行地处理，使[i, j]处最大子矩阵的面积是(right(i, j)-left(i, j))*height(i, j)。其中height统计当前位置及往上'1'的数量；left和right是高度是当前点的height值得左右边界，即是以当前点为中心，以height为高度向两边扩散的左右边界。

递推公式如下：
left(i, j) = max(left(i-1, j), cur_left);
right(i, j) = min(right(i-1, j), cur_right);
height(i, j) = height(i-1, j) + 1, if matrix[i][j]=='1';
height(i, j) = 0, if matrix[i][j]=='0'.
其中cur_left和cur_right的值由当前行决定，如果当前位置是'1'，则cur_left和cur_right都不变；如果当前位置是'0'，则cur_left就为当前位置的右侧，cur_right就为当前位置（因为左闭右开）。
时间复杂度：O(mn)
空间复杂度：O(n)

```c++
//此法不太理解
class Solution {
public:
      int maximalRectangle(vector<vector<char> > &matrix)
      {
          if (matrix.size()==0 || matrix[0].size()==0)
          {
              return 0;
          }
          //left和right是高度是当前点的height值得左右边界，即是以当前点为中心，以height为高度向两边扩散的左右边界。
          int m = matrix.size(), n = matrix[0].size();
          vector<int> height(n, 0);
          vector<int> left(n, 0);//记录上面几行元素左侧最大高度
          vector<int> right(n, n);//记录上面几行元素右侧最大高度
          int res = 0;
          for (int i =0;i<m;i++)
          {
              int cur_left = 0, cur_right = n;
              for (int j = 0;j<n;j++)
              {
                  height[j] = matrix[i][j] == '1' ? height[j] + 1 : 0;
                  left[j] = matrix[i][j] == '1' ? max(left[j], cur_left) : 0;
                  cur_left = matrix[i][j] == '1' ? cur_left : j+1;
              }
              for (int j = n-1;j>=0;j--)
              {
                  right[j] = matrix[i][j] == '1' ? min(right[j], cur_right) : n;
                  cur_right = matrix[i][j] == '1' ? cur_right : j;
              }
              for (int j = 0; j < n; ++j)
                  res = max(res, (right[j] - left[j]) * height[j]);
          }
          return res;
      }
};
```

