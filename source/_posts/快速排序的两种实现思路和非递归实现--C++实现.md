---
title: 快速排序的两种实现思路和非递归实现--C++实现
date: 2017-06-20 18:38:39
tags: [Algorithm]
categories: 
- Algorithm
---
# 思路一：
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;第一种是根据算法导论上的思想：取数组的最后一个元素为主元，i初始化为最低位元素的前一个位置，j指向遍历数组中待排序的元素，当j所指元素比主元小的时候i= i + 1，然后交换i和j所指的元素，j不断遍历，遇到小于主元的就进行交换，这样就能一直维持这样一个序列，i之前的元素(包括i所指元素本身)都是比主元小的元素，i到j之间都是比主元大的元素，j(包括j所指元素)之后都是待排序的元素，最后交换主元和第一个比主元大的元素就可以完成划分。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;算法导论具体介绍如下：
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;快速排序是基于分治模式处理的，对一个典型子数组A[p...r]排序的分治过程为三个步骤：
<!-- more -->
![这里写图片描述](http://img.blog.csdn.net/20160914113030607)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;具体实现快速排序的伪代码：

![这里写图片描述](http://img.blog.csdn.net/20160914113422605)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;具体例子分析详情：

![这里写图片描述](http://img.blog.csdn.net/20160914141747812)
![这里写图片描述](http://img.blog.csdn.net/20160914141848004)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;C++代码实现如下：

{% codeblock lang:c++ %}
//1、进行区域的划分
int getPartition(vector<int> &nums, int low, int height)
{
	int keyVal = nums[height];
	int i = low - 1;
	for (int j = low; j < height; j++)
	{
		
		if (nums[j] <= keyVal)
		{
			i = i + 1;
			swap(nums[i], nums[j]);
		}
	}
	swap(nums[i + 1], nums[height]);
	return i+1;
}

//2、递归调用划分区域函数，进行快速排序
void quickSort(vector<int> &nums,int low,int height) {

	if (low<height)
	{
		int mid = getPartition(nums, low, height);
		quickSort(nums, low, mid-1);
		quickSort(nums, mid + 1, height);
	}
}
{% endcodeblock %}


# 思路二：
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;第二种是严蔚敏的数据结构(C语言版)上的思想：取数组的第一个元素为主元，左(left)、右(height)两指针进行遍历，先右边开始边查找比主元小的（比主元大时height直接减减），找到就直接覆盖左边所指的元素，然后从左边开始查找比主元大的元素，找到就直接覆盖右边所指的元素，依次循环进行。最后low不小于height时把所取的主元付给low所指的位置，完成划分。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据结构(C语言版)具体介绍如下：

![这里写图片描述](http://img.blog.csdn.net/20160914145159315)
![这里写图片描述](http://img.blog.csdn.net/20160914145212018)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;具体实现快速排序的伪代码：

![这里写图片描述](http://img.blog.csdn.net/20160914145026674)
![这里写图片描述](http://img.blog.csdn.net/20160914145404379)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;具体例子分析详情：

![这里写图片描述](http://img.blog.csdn.net/20160914145636023)
![这里写图片描述](http://img.blog.csdn.net/20160914145651976)


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;C++代码实现如下：

{% codeblock lang:c++ %}
//1、进行区域的划分
int getPartition(vector<int> &nums, int low, int height)
{
	int keyVal = nums[low];
	while (low<height)
	{
		while (low < height&&nums[height] >= keyVal)
			height--;
		nums[low] = nums[height];
		while (low < height&&nums[low] <= keyVal)
			low++;
		nums[height] = nums[low];
	}
	nums[low] = keyVal;
	return low;
}

//2、递归调用划分区域函数，进行快速排序
void quickSort(vector<int> &nums,int low,int height) {

	if (low<height)
	{
		int mid = getPartition(nums, low, height);
		quickSort(nums, low, mid-1);
		quickSort(nums, mid + 1, height);
	}
}
{% endcodeblock %}




# 最后呈上快速排序的非递归实现：
-------------------

{% codeblock lang:c++ %}
void quickSortNonRecursive(vector<int> &nums, int low, int height)
{
	stack<int> s;
	if (low<height)
	{
		int mid = getPartition(nums, low, height);
		if (mid-1>low)
		{
			s.push(low);
			s.push(mid - 1);
		}
		if (mid+1<height)
		{
			s.push(mid + 1);
			s.push(height);
		}

		while (!s.empty())
		{
			int qHeight = s.top();
			s.pop();
			int pLow = s.top();
			s.pop();
			int pqMid = getPartition(nums, pLow, qHeight);
			if (pqMid - 1 > pLow)
			{
				s.push(pLow);
				s.push(pqMid - 1);
			}
			if (pqMid + 1 < qHeight)
			{
				s.push(pqMid + 1);
				s.push(qHeight);
			}
		}

	}
}
{% endcodeblock %}

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;其实经过测试非递归的算法比递归实现还要慢。 因为递归算法使用的栈由程序自动产生，栈中包含：函数调用时的参数和函数中的局部变量。如果局部变量很多或者函数内部又调用了其他函数，则栈会很大。每次递归调用都要操作很大的栈，效率自然会下降。而对于非递归算法，每次循环使用自己预先创建的栈，因此不管程序复杂度如何，都不会影响程序效率。但是对于上面的快速排序，由于局部变量只有一个mid，栈很小，所以效率并不比非递归实现的低。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;具体的关于快速排序的优化，提高快排效率将在后续文章给出。
