---
title: 快速排序优化
date: 2017-06-29 23:09:00
tags: Algorithm
categories: Algorithm
---

　　快速排序的思想是分治法，当每次划分时，算法若都能分成两个等长的子序列时，那么分治算法效率会达到最大。所以基准枢纽元的选择是很重要，选择基准的方式决定了分割后两个子序列的长度，进而对整个算法的效率产生决定性影响。
　　通常实现的快速排序没有经过充分考虑的选择那个枢纽元，只是将第一个或最后一个元素用作枢纽元。选择第一个元素作为枢纽元的程序例子见[快速排序的两种实现思路二](http://blog.taoaili999.cn/2017/06/20/QuickSort-implement-with-C++/),选择最后一个元素作为枢纽元的程序例子见[快速排序的两种实现思路一](http://blog.taoaili999.cn/2017/06/20/QuickSort-implement-with-C++/)

<!-- more-->

# 快速排序枢纽元选择的优化方法

## 方法一：随机选取枢纽元

　　如果输入序列是随机的，那么固定枢纽元为第一个或者最后一个这是可以接受的，但是如果输入是预排序的或者是反序的，那么固定枢纽元就产生一个劣质的分割，因为所有的元素不是被划入S1就是被划入S2。更有甚者，这种情况发生在所有的递归调用中。
　　实际上，如果第一个元素用作枢纽元而且输入是预先排序的，那么快速排序的时间复杂度为Θ(n^2)。然而，预排序的输入(或者有一大段预排序数据的输入)也是相当常见的，因此，使用第一个元素作为枢纽元的算法效率不是很高的。
　　随机选取枢纽元是相对安全的策略。由于枢轴的位置是随机的，那么产生的分割也不会总是会出现劣质的分割。在整个数组数字全相等时，仍然是最坏情况，时间复杂度是O(n^2）。实际上，随机化快速排序得到理论最坏情况的可能性仅为1/(2^n）。所以随机化快速排序可以对于绝大多数输入数据达到O(nlogn）的期望时间复杂度。

### 　　随机选取枢纽元算法：

```c++
//随机选取枢纽元
int getRandomPivot(int low,int height)
{
	//主要是利用随机数，随机的获取数组中的一个元素做为主元
	int size = height - low + 1;
	int index = low + rand() % size;
	return index;
}
//随机枢纽元放到第一个元素的getPartition划分过程
int getPartitionOptimize1(vector<int> &nums, int low, int height)
{
	//主要是利用随机数，随机的获取数组中的一个元素做为枢纽元
	int index =getRandomPivot(low,height);
  
	swap(nums[low], nums[index]);//交换第一个元素和随机枢纽元
	int keyVal = nums[low];
	while (low<height)
	{
		while (low<height&&nums[height]>=keyVal)
		{
			height--;
		}
		nums[low] = nums[height];
		while (low<height&&nums[low]<=keyVal)
		{
			low++;
		}
		nums[height] = nums[low];
	}
	nums[low] = keyVal;
	return low;
}
//随机枢纽元放到最后一个元素的getPartition划分过程
int getPartitionOptimizer2(vector<int> &nums, int low, int height)
{
	//主要是利用随机数，随机的获取数组中的一个元素做为枢纽元
	int index =getRandomPivot(low,height);

	swap(nums[height], nums[index]);//交换最后一个元素和随机枢纽元
	int keyVal = nums[height];
	int i = low - 1;
	for (int j = low;j<height;j++)
	{
		if (nums[j]<=keyVal)
		{
			i = i + 1;
			swap(nums[j], nums[i]);
		}
	}
	swap(nums[i + 1], nums[height]);
	return i+1;
}
```

## 方法二：三数取中分割法选取枢纽元

　　虽然随机选取枢轴时，减少出现不好分割的几率，但是最坏情况下还是O(n^2），要缓解这种情况，就引入了三数取中选取枢纽，一组N个数的中值是第[N/2]个最大的数。枢纽元的最好的选择是数组的中值。可是，这很难算出来，并且会明显减慢快速排序的速度。这样的中值的估计可以通过随机选取三个元素并用它们的中值作为枢纽元而得到。事实上，随机性并没有多大的帮助，因此一般的做法是使用左端、右端和中心位置上的三个元素的中值作为枢纽元。显然使用三数中值分割法消除了预排序输入的不好情形。

### 　　三数取中分割法枢纽元算法：

```c++
//三数取中分割法:通过确定nums[low], nums[mid], nums[height]三者之中的那个第二大的元素为枢纽元时，便能尽最大限度保证快速排序算法不会出现O（N ^ 2）的最坏情况。。
void getMedianOfThreePivot(vector<int> &nums, int low, int height)
{
	//主要是利用三元区中值，随机的获取数组中的一个元素做为枢纽元
	int mid = (low + height) / 2;
	//首先把三元素中的最小的元素交换到中间
	if (nums[low] < nums[mid])
		swap(nums[low], nums[mid]);
	if (nums[height] < nums[mid])
		swap(nums[height], nums[mid]);
	//在比较两个较大的元素，把最小的放到最左边位置，即为三数中值
	if (nums[height] < nums[low])
		swap(nums[height], nums[low]);
}
//三数取中分割法枢纽元放到第一个元素的getPartition划分过程
int getPartitionOptimize1(vector<int> &nums, int low, int height)
{
	getMedianOfThreePivot(nums,low,height);//确定枢纽元

	int keyVal = nums[low];
	while (low < height)
	{
		while (low < height&&nums[height] >= keyVal)
		{
			height--;
		}
		nums[low] = nums[height];
		while (low < height&&nums[low] <= keyVal)
		{
			low++;
		}
		nums[height] = nums[low];
	}
	nums[low] = keyVal;
	return low;
}
//三数取中分割法枢纽元放到最后一个元素的getPartition划分过程
int getPartitionOptimizer4(vector<int> &nums, int low, int height)
{
	getMedianOfThreePivot(nums,low,height);//确定枢纽元

	int keyVal = nums[height];
	int i = low - 1;
	for (int j = low; j < height; j++)
	{
		if (nums[j] <= keyVal)
		{
			i = i + 1;
			swap(nums[j], nums[i]);
		}
	}
	swap(nums[i + 1], nums[height]);
	return i + 1;
}



```



# 枢纽元确定后递归快排函数优化方法

## 方法一：当待排序序列的长度分割到一定大小后，使用插入排序

　　对于很小和部分有序的数组，快排不如插排好。当待排序序列的长度分割到一定大小后，继续分割的效率比插入排序要差，此时可以使用插排而不是快排．截止范围：待排序序列长度N < 10，进行插入排序，而不是继续划分快排。

```c++
//利用插入排序优化
void insertSort(vector<int> &nums, int low, int height)
{
	for (int i = low; i <= height; i++)
	{
		int key = nums[i];
		int j = i - 1;
		while (j >= low && key < nums[j])
		{
			nums[j + 1] = nums[j];
			j--;
		}
		nums[j + 1] = key;
	}
}
//递归快排函数通过加入插入排序进行优化
void quickSortOptimize(vector<int> &nums, int low, int height) 
{
	if (height - low + 1 < 10)
	{
		insertSort(nums, low, height);
		return;
	}
	else
	{
		int mid = getPartitionOptimize1(nums, low, height);
		quickSort(nums, low, mid - 1);
		quickSort(nums, mid + 1, height);
	}
}
```

## 方法二：在一次分割结束后，可以把与枢纽元相等的元素聚在一起，继续下次分割时，不用再对与枢纽元相等元素分割

### 　举例：

```c++
举例：待排序序列 1 4 6 7 6 6 7 6 8 6
三数取中选取枢轴：下标为4的数6
转换后，待分割序列：6 4 6 7 1 6 7 6 8 6	
枢纽元key：6
本次划分后，未对与key元素相等处理的结果：1 4 6 6 7 6 7 6 8 6
下次的两个子序列为：1 4 6 和 7 6 7 6 8 6
本次划分后，对与key元素相等处理的结果：1 4 6 6 6 6 6 7 8 7
下次的两个子序列为：1 4 和 7 8 7
经过对比，我们可以看出，在一次划分后，把与key相等的元素聚在一起，能减少迭代次数，效率会提高不少
```

###  　具体实现过程分为两步：

```c 
第一步，在划分过程中，把与枢纽元key相等元素放入数组的两端
第二步，划分结束后，把与枢纽元key相等的元素移到枢纽元周围
```

### 　具体代码实现：

　　　枢纽元通过三数取中分割法后放在第一个元素，此法是从左右两边分别向中间靠拢，所以和枢纽元相等的值会出现在枢纽元左右两边，因此需要左右两边都进行相等枢纽元的聚集。

```c++
vector<int>  getPartitionGatherKey(vector<int> &nums, int low, int height)
{
	getMedianOfThreePivot(nums, low, height);//转换枢纽元
	int tmpLow = low;
	int tmpHeight = height;

	int left = low;
	int right = height;
	int leftCommonNum = 0;//记录左侧和枢纽元相等的元素个数
	int rightCommonNum = 0;//记录右侧和枢纽元相等的元素个数

	//第一步：划分的过程中把与枢纽元相等的元素分散到数组的两端
	int keyVal = nums[low];
	while (low < height)
	{
		while (low < height && nums[height] >= keyVal)
		{
			if (nums[height] == keyVal)
			{
				swap(nums[height], nums[right]);//相等元素放到右端
				--right;
				++rightCommonNum;
			}
			--height;
		}
		nums[low] = nums[height];
		while (low < height && nums[low] <= keyVal)
		{
			if (nums[low] == keyVal)
			{
				swap(nums[low], nums[left]);//相等元素放到左端
				++left;
				++leftCommonNum;
			}
			++low;
		}
		nums[height] = nums[low];
	}
	nums[low] = keyVal;

	//第二步：划分结束后，把与keyVal相等的元素移到枢纽轴（也就是枢纽元素）的两边
	for (int i = low -1,j = tmpLow;j < left;j++)
	{
		if (nums[i] != keyVal)
			swap(nums[j], nums[i]);
		--i;
	}
	for (int i = low + 1, j = tmpHeight; j > right; j--)
	{
		if (nums[i] != keyVal)
			swap(nums[j], nums[i]);
		++i;
	}
    //计算划分区域的索引
	int partitionLeftIndex = low - 1 - leftCommonNum;
	int partitionRighttIndex = low + 1 + rightCommonNum;
	vector<int> partitionIndex;
	partitionIndex.push_back(partitionLeftIndex);
	partitionIndex.push_back(partitionRighttIndex);

	return partitionIndex;
}
void quickSortGatherKey(vector<int> &nums, int low, int height)
{
	if (height - low + 1 < 10)
	{
		insertSort(nums, low, height);
		return;
	}
	else
	{
		vector<int> partitionIndex = getPartitionGatherKey(nums, low, height);
		quickSortGatherKey(nums, low, partitionIndex[0]);
		quickSortGatherKey(nums, partitionIndex[1], height);
	}
}
```

　　　枢纽元通过三数取中分割法后放在最后一个元素，此法枢纽元左边都是小于等于枢纽元的元素，右边都是大于枢纽元的元素，所以和枢纽元相等的值只会出现在枢纽元左两边，因此只需要左边进行相等枢纽元的聚集。

```c++
vector<int> getPartitionGatherKey(vector<int> &nums, int low, int height)
{
	getMedianOfThreePivot(nums, low, height);//
	swap(nums[height], nums[low]);//三数取中分割法后枢纽元在最左端，需放到最后一个元素处
	int tmpLow = low;
	int left = low;

	int leftCommonNum = 0;//记录左侧和枢纽元相等的元素个数
	
	//第一步：划分的过程中把与枢纽元相等的元素分散到数组的左端
	int keyVal = nums[height];
	int i = low - 1;
	for (int j = low; j < height; j++)
	{
		if (nums[j] <= keyVal)
		{
			i = i + 1;
			swap(nums[j], nums[i]);
			if (nums[i] == keyVal)
			{
				swap(nums[left], nums[i]);
				++left;
				++leftCommonNum;
			}
		}
	}
	swap(nums[i + 1], nums[height]);

	//第二步：划分结束后，把与keyVal相等的元素移到枢纽轴（也就是枢纽元素）的左边
	for (int k = i,j = tmpLow;j < left;j++)
	{
		if (nums[k] != keyVal)
		{
			swap(nums[k], nums[j]);
		}
		--k;
	}
	int partitionLeftIndex = i - leftCommonNum;
	int partitionRighttIndex = i + 2;
	vector<int> partitionIndex;
	partitionIndex.push_back(partitionLeftIndex);
	partitionIndex.push_back(partitionRighttIndex);
	return partitionIndex;
}
void quickSortGatherKey(vector<int> &nums, int low, int height)
{
	if (height - low + 1 < 10)
	{
		insertSort(nums, low, height);
		return;
	}
	else
	{
		vector<int> partitionIndex = getPartitionGatherKey(nums, low, height);
		quickSortGatherKey(nums, low, partitionIndex[0]);
		quickSortGatherKey(nums, partitionIndex[1], height);
	}
}
```

<font size=6>**　　综上所述，以及对大量数据的测试可以得到最好的快排组合是：<font size=16  color=#0099ff >三数取中+插排+聚集相等元素</font>,它和STL中的Sort函数效率差不多**</font>

