---
title: 4. Median of Two Sorted Arrays
date: 2017-06-27 11:03:23
tags: LeetCode
categories: LeetCode			
---

# 4. Median of Two Sorted Arrays

There are two sorted arrays nums1 and nums2 of size m and n respectively.
Find the median of the two sorted arrays.The overall run time complexity should be O(log(m + n)).

**Example 1:**
nums1 = [1, 3]
nums2 = [2]
The median is 2.0

**Example2:**
nums1 = [1, 2]
nums2 = [3, 4]
The median is(2 + 3) / 2 = 2.5

<!--more-->

## 题意：

　　给定两个排序数组nums1和nums2，大小分别是m和n，找到这两个排序数组的中位数，要求时间复杂度是O(log(m + n))。

## 思路：

　　该方法的核心是将原问题转变成一个寻找第k小数的问题（假设两个原序列升序排列），这样中位数实际上是第(m + n) / 2小的数。所以只要解决了第k小数的问题，原问题也得以解决。
　　首先假设数组A和B的元素个数都大于k / 2，我们比较A[k / 2 - 1]和B[k / 2 - 1]两个元素，这两个元素分别表示A的第k / 2小的元素和B的第k / 2小的元素。这两个元素比较共有三种情况： > 、 < 和 = 。如果A[k / 2 - 1]<B[k / 2 - 1]，这表示A[0]到A[k / 2 - 1]的元素都在A和B合并之后的前k小的元素中。换句话说，A[k / 2 - 1]不可能大于两数组合并之后的第k小值，所以我们可以将其抛弃。
　　证明也很简单，可以采用反证法。假设A[k / 2 - 1]大于合并之后的第k小值，我们不妨假定其为第（k + 1）小值。由于A[k / 2 - 1]小于B[k / 2 - 1]，所以B[k / 2 - 1]至少是第（k + 2）小值。但实际上，在A中至多存在k / 2 - 1个元素小于A[k / 2 - 1]，B中也至多存在k / 2 - 1个元素小于A[k / 2 - 1]，所以小于A[k / 2 - 1]的元素个数至多有k / 2 + k / 2 - 2，小于k，这与A[k / 2 - 1]是第（k + 1）的数矛盾。
　　当A[k / 2 - 1]>B[k / 2 - 1]时存在类似的结论。
　　当A[k / 2 - 1] = B[k / 2 - 1]时，我们已经找到了第k小的数，也即这个相等的元素，我们将其记为m。由于在A和B中分别有k / 2 - 1个元素小于m，所以m即是第k小的数。(这里可能有人会有疑问，如果k为奇数，则m不是中位数。这里是进行了理想化考虑，在实际代码中略有不同，是先求k / 2，然后利用k - k / 2获得另一个数。)
　　通过上面的分析，我们即可以采用递归的方式实现寻找第k小的数。此外我们还需要考虑几个边界条件：
　　　1. 如果A或者B为空，则直接返回B[k - 1]或者A[k - 1]；
　　　2. 如果k为1，我们只需要返回A[0]和B[0]中的较小值；
　　　3. 如果A[k / 2 - 1] = B[k / 2 - 1]，返回其中一个；

```c++
class Solution {
public:
	double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
		int lenA = nums1.size();
		int lenB = nums2.size();
		if (lenA == 0 && lenB == 0)
		{
			return 0;
		}
		int total = lenA + lenB;
		if (total & 0x1)
		{
			return getKth(nums1, 0, lenA, nums2, 0, lenB, total / 2 + 1);//如果为奇数，寻找第total/2+1小元素
		}
		else//如果为偶数，寻找第total/2+1小和total/2 小元素
		{
			double d1 = getKth(nums1, 0, lenA, nums2, 0, lenB, total / 2 + 1);
			double d2 = getKth(nums1, 0, lenA, nums2, 0, lenB, total / 2);
			return (d1 + d2) / 2.0;
		}
	}

	double getKth(vector<int> &nums1,int startA, int lenA, vector<int> &nums2,int startB, int lenB, int totalHalf)
	{
		if (lenB < lenA)//把短的数組一定放在前面，便于后面计算
		{
			return getKth(nums2,startB, lenB, nums1,startA, lenA, totalHalf);
		}
		if (lenA == 0)
		{
			return nums2[startB + totalHalf - 1];
		}
		if (totalHalf == 1)
		{
			return min(nums1[startA], nums2[startB]);
		}

		int midK1 = min(lenA, totalHalf / 2);
		int midK2 = totalHalf - midK1;

		if (nums1[startA + midK1 - 1] < nums2[startB + midK2 - 1])
			return getKth(nums1, startA + midK1, lenA - midK1, nums2, startB, lenB, totalHalf - midK1);
		else if (nums1[startA + midK1 - 1] > nums2[startB + midK2 - 1])
			return getKth(nums1, startA, lenA, nums2, startB + midK2, lenB - midK2, totalHalf - midK2);
		else
			return nums1[startA + midK1 - 1];
	}
};

int main()
{
	vector<int> nums1 = { 1,5,8 };
	vector<int> nums2 = { 2,6,7 };
	Solution s;
	double res = s.findMedianSortedArrays(nums1, nums2);
	return 0;
}
```

---------------------------------------------------
#### 　Java Code：
```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int len1 = nums1.length;
        int len2 = nums2.length;
        if (len1 == 0 && len2 == 0) {
            return 0;
        }

        int total = len1 + len2;
        if ((total & 1) == 1) {
            return getKth(nums1, 0, len1, nums2, 0, len2, total / 2 + 1);
        } else {
            double pre = getKth(nums1, 0, len1, nums2, 0, len2, total / 2);
            double after = getKth(nums1, 0, len1, nums2, 0, len2, total / 2 + 1);
            return (pre + after) / 2;
        }
    }

    public int getKth(int[] nums1, int start1, int len1, int[] nums2, int start2, int len2, int k) {
        if (len2 < len1) {
            return getKth(nums2, start2, len2, nums1, start1, len1, k);
        }

        if (len1 == 0) {
            return nums2[start2 + k - 1];
        }

        if (k == 1) {
            return Math.min(nums1[start1], nums2[start2]);
        }

        int elementNum1 = Math.min(len1, k / 2);
        int elementNum2 = k - elementNum1;

        if (nums1[start1 + elementNum1 - 1] < nums2[start2 + elementNum2 -1]) {
            return getKth(nums1, start1 + elementNum1, len1 - elementNum1, nums2, start2, len2, k - elementNum1);
        } else if (nums1[start1 + elementNum1 - 1] > nums2[start2 + elementNum2 -1]) {
            return getKth(nums1, start1, len1, nums2, start2 + elementNum2, len2 - elementNum2, k - elementNum2);
        } else {
            return nums1[start1 + elementNum1 - 1];
        }

    }
}

public class MainClass {
    public static int[] stringToIntegerArray(String input) {
        input = input.trim();
        input = input.substring(1, input.length() - 1);
        if (input.length() == 0) {
          return new int[0];
        }

        String[] parts = input.split(",");
        int[] output = new int[parts.length];
        for(int index = 0; index < parts.length; index++) {
            String part = parts[index].trim();
            output[index] = Integer.parseInt(part);
        }
        return output;
    }

    public static String doubleToString(double input) {
        return new DecimalFormat("0.00000").format(input);
    }

    public static void main(String[] args) throws IOException {
        BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
        String line;
        while ((line = in.readLine()) != null) {
            int[] nums1 = stringToIntegerArray(line);
            line = in.readLine();
            int[] nums2 = stringToIntegerArray(line);

            double ret = new Solution().findMedianSortedArrays(nums1, nums2);

            String out = doubleToString(ret);

            System.out.print(out);
        }
    }
}
```
