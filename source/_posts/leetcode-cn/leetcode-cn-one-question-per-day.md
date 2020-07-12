---
title: LeetCode-CN 每日一题汇总
date: 2020-06-24 11:32:51
tags: LeetCode
categories: LeetCode
---



Goog Good Study, Day Day Up......
<!--more-->

# 2020-07
日期  | 题目  | 题解  |  难度
--|--------------------------------------|-------------------------------------------------------------------|-------------
1 | [718. 最长重复子数组][07f8cffe]        | [动态规划][5f879b2e]           |  ☆ ☆ ☆
2 | [378. 有序矩阵中第K小的元素][7cbac919] | [二分查找 + 归并排序(利用小顶堆进行归并)][34a1a05e]           |  ☆ ☆ ☆
3 | [108. 将有序数组转换为二叉搜索树][6236a653]  | [中序遍历，总是选择中间位置左边的数字作为根节点][dc3f6a2d]  |  ☆
4 | [32. 最长有效括号][bcccf16e]  |  [栈(栈内元素是包含括号**字符和索引的包装类**)][13b4bd58] |  ☆ ☆
5 | [44. 通配符匹配][15adc358] | [动态规划][4bb173d5]  |  ☆ ☆ ☆ ☆ ☆
6 | [63. 不同路径 II][e9541b66] | [动态规划][cdb1223e]  |  ☆ ☆ ☆
7 | [112. 路径总和][494630ab]  | [递归][4dfbeaef]  |  ☆ ☆
8 | [面试题 16.11. 跳水板][cd753bf9]  | [数学方法枚举][6c976a8b]  |  ☆
9 | [面试题 17.13. 恢复空格][f4dc6a4c] | [动态规划 + **字典树(Trie)**][483d76fd]  | ☆ ☆ ☆ ☆
10| [309. 最佳买卖股票时机含冷冻期][6bca6544] | [动态规划(会用状态机状态转换的思想，对股票的收益和不同操作状态进行动态转换)][fa1c1940]  |  ☆ ☆ ☆ ☆ ☆
11| [315. 计算右侧小于当前元素的个数][9eb494a7]| [归并排序计算逆序数 + 索引数组][daa48538]  [submissions][20118eb1] |  ☆ ☆ ☆ ☆ ☆
12| [174. 地下城游戏][5288cad5]  | [动态规划(此题正向进行动态规划很难确定两个预期值的最优，采用逆向，从数组右下角开始)][88c6db87]  |  ☆ ☆ ☆ ☆ ☆

  [07f8cffe]: https://leetcode-cn.com/problems/maximum-length-of-repeated-subarray/ "最长重复子数组"
  [5f879b2e]: https://leetcode-cn.com/problems/maximum-length-of-repeated-subarray/solution/zui-chang-zhong-fu-zi-shu-zu-by-leetcode-solution/ "最长重复子数组题解"

  [7cbac919]: https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix/ "有序矩阵中第K小的元素"
  [34a1a05e]: https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix/solution/you-xu-ju-zhen-zhong-di-kxiao-de-yuan-su-by-leetco/ "有序矩阵中第K小的元素题解"

  [6236a653]: https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/ "将有序数组转换为二叉搜索树"
  [dc3f6a2d]: https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/solution/jiang-you-xu-shu-zu-zhuan-huan-wei-er-cha-sou-s-33/ "将有序数组转换为二叉搜索树题解"

  [bcccf16e]: https://leetcode-cn.com/problems/longest-valid-parentheses/ "最长有效括号"
  [13b4bd58]: https://leetcode-cn.com/problems/longest-valid-parentheses/solution/zui-chang-you-xiao-gua-hao-by-leetcode-solution/ "最长有效括号题解"

  [15adc358]: https://leetcode-cn.com/problems/wildcard-matching/ "通配符匹配"
  [4bb173d5]: https://leetcode-cn.com/problems/wildcard-matching/solution/tong-pei-fu-pi-pei-by-leetcode-solution/ "通配符匹配题解"

  [e9541b66]: https://leetcode-cn.com/problems/unique-paths-ii/ "不同路径 II"
  [cdb1223e]: https://leetcode-cn.com/problems/unique-paths-ii/solution/jian-dan-dpbi-xu-miao-dong-by-sweetiee/ "不同路径 II题解"

  [494630ab]: https://leetcode-cn.com/problems/path-sum/ "路径总和"
  [4dfbeaef]: https://leetcode-cn.com/problems/path-sum/solution/lu-jing-zong-he-by-leetcode-solution/ "路径总和题解"

  [cd753bf9]: https://leetcode-cn.com/problems/diving-board-lcci/ "跳水板"
  [6c976a8b]: https://leetcode-cn.com/problems/diving-board-lcci/solution/tiao-shui-ban-by-leetcode-solution/ "跳水板题解"

  [f4dc6a4c]: https://leetcode-cn.com/problems/re-space-lcci/ "恢复空格"
  [483d76fd]: https://leetcode-cn.com/problems/re-space-lcci/solution/jian-dan-dp-trieshu-bi-xu-miao-dong-by-sweetiee/ "恢复空格题解"

  [6bca6544]: https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/ "最佳买卖股票时机含冷冻期"
  [fa1c1940]: https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/solution/zui-jia-mai-mai-gu-piao-shi-ji-han-leng-dong-qi-4/ "最佳买卖股票时机含冷冻期题解"

  [9eb494a7]: https://leetcode-cn.com/problems/count-of-smaller-numbers-after-self/ "计算右侧小于当前元素的个数"
  [daa48538]: https://leetcode-cn.com/problems/count-of-smaller-numbers-after-self/solution/gui-bing-pai-xu-suo-yin-shu-zu-python-dai-ma-java-/ "计算右侧小于当前元素的个数题解"
  [20118eb1]: https://leetcode-cn.com/submissions/detail/86899179/ "计算右侧小于当前元素的个数题解submissions"

  [5288cad5]: https://leetcode-cn.com/problems/dungeon-game/ "地下城游戏"
  [88c6db87]: https://leetcode-cn.com/problems/dungeon-game/solution/di-xia-cheng-you-xi-by-leetcode-solution/ "地下城游戏题解"





---

# 2020-06
日期  | 题目  | 题解  |  难度
--|--------------------------------------|-------------------------------------------------------------------|-------------
21| [124. 二叉树中的最大路径和][44004149]  | [递归，要理解最大路径和，节点的最大贡献值的区别][e3010ff0]            |  ☆ ☆
22| [面试题 16.18. 模式匹配][76d40f3f]    | [根据模式枚举][1c9f7a75]                                           |  ☆ ☆ ☆ ☆
23| [67. 二进制求和][d7653bc8]            | [末尾对齐，短序列遍历过程空位取0,逐位相加][edfdc676]                 |  ☆
24| [16. 最接近的三数之和][c40f64d6]      | [排序 + 双指针][8f8b5b96]                                          |  ☆
25| [139. 单词拆分][3a83ce19]            | [动态规划，刚刚开始用DFS实现，运行超时，需理解动态规划思想][6039d227]  |  ☆ ☆ ☆
26| [面试题 02.01. 移除重复节点][c3bd3cf9]| [哈希表][ca628dd0]                                                 |  ☆
27| [41. 缺失的第一个正数][792a5804]      | [哈希表(通过数组值对应原数组下标构建hash表，两种方式：置换 + 标记为负数)][5607421a]  |  ☆ ☆ ☆
28| [209. 长度最小的子数组][c81dfc01]     |  [双指针][5f17fcea]                                               |  ☆ ☆
29| [215. 数组中的第K个最大元素][bf3f39ea] | [基于快速排序的选择方法][4c6b7c9a]                                 |  ☆ ☆ ☆
30| [剑指 Offer 09. 用两个栈实现队列][3191ca41] | [双栈][678be353]                                             | ☆

[44004149]: https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/ "二叉树中的最大路径和"
[e3010ff0]: https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/solution/er-cha-shu-zhong-de-zui-da-lu-jing-he-by-leetcode-/ "二叉树中的最大路径和题解"


[76d40f3f]: https://leetcode-cn.com/problems/pattern-matching-lcci/ "面试题 16.18. 模式匹配"
[1c9f7a75]: https://leetcode-cn.com/problems/pattern-matching-lcci/solution/mo-shi-pi-pei-by-leetcode-solution/  "面试题 16.18. 模式匹配题解"


[d7653bc8]: https://leetcode-cn.com/problems/add-binary/ "67. 二进制求和"
[edfdc676]: https://leetcode-cn.com/problems/add-binary/solution/hua-jie-suan-fa-67-er-jin-zhi-qiu-he-by-guanpengch/ "二进制求和题解"


[c40f64d6]: https://leetcode-cn.com/problems/3sum-closest/ "16. 最接近的三数之和"
[8f8b5b96]: https://leetcode-cn.com/problems/3sum-closest/solution/zui-jie-jin-de-san-shu-zhi-he-by-leetcode-solution/ "最接近的三数之和题解"


[3a83ce19]: https://leetcode-cn.com/problems/word-break/ "139. 单词拆分"
[6039d227]: https://leetcode-cn.com/problems/word-break/solution/dan-ci-chai-fen-by-leetcode-solution/ "单词拆分题解"


[c3bd3cf9]: https://leetcode-cn.com/problems/remove-duplicate-node-lcci/ "面试题 02.01. 移除重复节点"
[ca628dd0]: https://leetcode-cn.com/problems/remove-duplicate-node-lcci/solution/yi-chu-zhong-fu-jie-dian-by-leetcode-solution/ "移除重复节点题解"


[792a5804]: https://leetcode-cn.com/problems/first-missing-positive/ "缺失的第一个正数"
[5607421a]: https://leetcode-cn.com/problems/first-missing-positive/solution/que-shi-de-di-yi-ge-zheng-shu-by-leetcode-solution/ "缺失的第一个正数题解"


[c81dfc01]: https://leetcode-cn.com/problems/minimum-size-subarray-sum/ "长度最小的子数组"
[5f17fcea]: https://leetcode-cn.com/submissions/detail/83483608/ "长度最小的子数组题解"


[bf3f39ea]: https://leetcode-cn.com/problems/kth-largest-element-in-an-array/ "数组中的第K个最大元素"
[4c6b7c9a]: https://leetcode-cn.com/problems/kth-largest-element-in-an-array/solution/shu-zu-zhong-de-di-kge-zui-da-yuan-su-by-leetcode-/ "数组中的第K个最大元素题解"


[3191ca41]: https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/ "用两个栈实现队列"
[678be353]: https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/solution/mian-shi-ti-09-yong-liang-ge-zhan-shi-xian-dui-l-3/ "用两个栈实现队列题解"
