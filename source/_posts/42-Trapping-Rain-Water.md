---
title: 42. Trapping Rain Water
date: 2017-07-20 21:20:35
tags: LeetCode
categories: LeetCode
---

# 42. Trapping Rain Water(hasDone)

Given n non - negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

For example,
Given[0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1], return 6.
![42-rainwatertrap](images/42-rainwatertrap.png)
The above elevation map is represented by array[0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1].In this case, 6 units of rain water(blue section) are being trapped.

<!--more-->

## 题意：

　　假设n个非负整数代表一个海拔图，每个条的宽度为1，计算它能在雨后捕获多少水。具体捕水例子见上图。

## 思路：

　　主要是抓住存水的条件，当元素的右侧元素比其小的时候，说明左侧有可能存水，只要后面存在大于等于此元素的元素值，所以从左往右遍历，一次查找可能存水的区域，叠加，但是还有种情况就是从左到右遍历完成后，最右侧剩余一段都小于某个元素值的区域段，这个时候没有把此区域存水的值叠加进去，所以需要对剩余的元素进行从右往左遍历，一次叠加存水值。
　　其中主要的判断条件 就是找到往后遍历的比第一个元素大的值，这个时候肯定会存水。

```c++
int trap(vector<int>& height) {
    vector<int> preHeightVal;
    int sum = 0;
    for(int i = 0; i<height.size();i++){//从前往后遍历
       if(preHeightVal.empty() || height[i]<preHeightVal[0]){
             preHeightVal.push_back(height[i]); 
        }
	   else {
		   while (!preHeightVal.empty())
		   {
			   sum += (preHeightVal[0] - preHeightVal.back());
			   preHeightVal.erase(preHeightVal.end() - 1);
		   }
		   preHeightVal.push_back(height[i]);

	   }
    }
    vector<int> lastHeightVal;
    for(int i = preHeightVal.size()-1;i>=0;i--)//如果preHeightVal.size()不为零，说明还剩余元素小于preHeightVal的第一个元素，但是有可能存水，从后往前遍历
    {
        if(lastHeightVal.empty() || preHeightVal[i]<lastHeightVal[0]){//从后往前遍历
             lastHeightVal.push_back(preHeightVal[i]);
        }
		else{
			while(!lastHeightVal.empty())
			{
				 sum +=  (lastHeightVal[0] - lastHeightVal.back());
				 lastHeightVal.erase(lastHeightVal.end()-1);  
			} 
			lastHeightVal.push_back(preHeightVal[i]);
		}

    }
	return sum;
}
```

