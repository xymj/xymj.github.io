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
13| [350. 两个数组的交集 II][b5f3f15e] | [排序 + 双指针，两数组排序后，根据各个数组指针所指值大小来移动求交集][8de02473] |  ☆ ☆
14| [120. 三角形最小路径和][fc44f754]  | [动态规划(重点是从三角形的底部开始转移，到顶部结束，转移的过程中计算最小和)][72ec47df]  |  ☆ ☆ ☆
15| [96. 不同的二叉搜索树][1a194028]  | [动态规划(当前节点数的不同二叉搜索数个数 = 左子树不同二叉搜索树个数 * 右子树不同二叉搜索树个数)][1bf529f5]  |   ☆ ☆ ☆
16| [785. 判断二分图][7e129613]  | [节点染色 + 广度/深度优先搜索遍历图(二分图节点分为两种颜色红、绿，如果相邻节点颜色同时是红和绿，肯定不是二分图)][7506af74]  |  ☆ ☆ ☆ ☆
17| [35. 搜索插入位置][aac30c0f]  | [二分查找][34c42412]  |  ☆
18| [97. 交错字符串][e61901e0]  | [动态规划(定义 dp(i)(j) 表示s1​的前i个元素和s2​的前j个元素是否能交错组成s3​的前i+j 个元素)][760fd551]|   ☆ ☆ ☆ ☆ ☆
19| [312. 戳气球][31c506da]  | [动态规划(def(i,j)函数的定义则为不戳破i与j，仅戳破i与j之间的气球我们能得到的最大金币数，如此划分状态转移方程def(i,j) = max{ def(i,k) + def(k,j)+nums\[i\]\[j\]\[k\]}\|i<k<j的实现,其中nums\[i\]\[j\]\[k\]为戳破气球k时我们能得到的金币数，因为def(i,j)表示戳破i到j之间的气球，自然包括k)][a178d2fb]  |  ☆ ☆ ☆ ☆ ☆ ☆
20| [167. 两数之和 II - 输入有序数组][b14d7542]  |[ 双指针][4fbfd020]  |  ☆   
21| [95. 不同的二叉搜索树 II][15aeb9c1]  | [递归(根据根节点，递归求解左右两边节点分别所能构的二叉搜索数集合，组合左右集合，构建当前根节点二叉搜索数)][29d2b07c]  |   ☆ ☆ ☆
22| [剑指 Offer 11. 旋转数组的最小数字][d1c3ca0e]  |  [二分法(根据mid节点的值与right节点值大小的比较来移动left和right指针)][79c44736]  |  ☆ ☆
23| [64. 最小路径和][8d44eb25]  | [动态规划][839d5e7d]  |  ☆ ☆
24| [1025. 除数博弈][5850a1ff]  | [数学问题，递推找规律(N奇数的时候Alice（先手）必败，N为偶数的时候Alice必胜)][f6c1a0e1]  |  ☆ ☆
25| [410. 分割数组的最大值][3f190060]  | [动态规划(dp\[i\]\[j\]表示将数组的前i个数分割为j段所能得到的最大连续子数组和的最小值)][54d569a1]   |  ☆ ☆ ☆ ☆ ☆
26| [329. 矩阵中的最长递增路径][7976e98d]  | [记忆化深度优先搜索(需要有一个二维数组记录每个节点能走的最远路径大小)][80d272a7]  |   ☆ ☆ ☆ ☆ ☆
27| [392. 判断子序列][b5e5b225]  |[ 双指针][d646d4c4]  |  ☆
28| [104. 二叉树的最大深度][6f48a2d0]  | [递归][1fd2273a]   |   ☆
29| [LCP 13. 寻宝][ef3b2276]  | [状态压缩动态规划(题解未看懂)][5620f3ca]  |  ☆ ☆ ☆ ☆ ☆ ☆ ☆
30| [343. 整数拆分][42b1ab82]  | [动态规划(dp\[i\]表示将正整数i拆分成至少两个正整数的和之后，这些正整数的最大乘积。将i拆分成j和i−j的和，如果i−j不再拆分成多个正整数，此时的乘积是j×(i−j)；如果i−j继续拆分成多个正整数，此时的乘积是j×dp\[i−j\])][760c099f]   |  ☆ ☆ ☆ ☆
31| [面试题 08.03. 魔术索引][08950398]  | [单次遍历判断索引和索引对应的元素值][bdb57086]  |  ☆

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

  [b5f3f15e]: https://leetcode-cn.com/problems/intersection-of-two-arrays-ii/ "两个数组的交集 II"
  [8de02473]: https://leetcode-cn.com/problems/intersection-of-two-arrays-ii/solution/liang-ge-shu-zu-de-jiao-ji-ii-by-leetcode-solution/ "两个数组的交集 II题解"

  [fc44f754]: https://leetcode-cn.com/problems/triangle/ "三角形最小路径和"
  [72ec47df]: https://leetcode-cn.com/submissions/detail/87563691/ "三角形最小路径和题解"

  [1a194028]: https://leetcode-cn.com/problems/unique-binary-search-trees/ "不同的二叉搜索树"
  [1bf529f5]: https://leetcode-cn.com/problems/unique-binary-search-trees/solution/bu-tong-de-er-cha-sou-suo-shu-by-leetcode-solution/ "不同的二叉搜索树题解"

  [7e129613]: https://leetcode-cn.com/problems/is-graph-bipartite/ "判断二分图"
  [7506af74]: https://leetcode-cn.com/problems/is-graph-bipartite/solution/pan-duan-er-fen-tu-by-leetcode-solution/ "判断二分图题解"

  [aac30c0f]: https://leetcode-cn.com/problems/search-insert-position/ "搜索插入位置"
  [34c42412]: https://leetcode-cn.com/problems/search-insert-position/solution/sou-suo-cha-ru-wei-zhi-by-leetcode-solution/ "搜索插入位置题解"

  [e61901e0]: https://leetcode-cn.com/problems/interleaving-string/ "交错字符串"
  [760fd551]: https://leetcode-cn.com/problems/interleaving-string/solution/jiao-cuo-zi-fu-chuan-by-leetcode-solution/ "交错字符串题解"

  [31c506da]: https://leetcode-cn.com/problems/burst-balloons/ "戳气球"
  [a178d2fb]: https://leetcode-cn.com/problems/burst-balloons/solution/chao-xiang-xi-hui-su-dao-fen-zhi-dao-dp-by-niu-you/ "戳气球题解"

  [b14d7542]: https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/ "两数之和 II - 输入有序数组"
  [4fbfd020]: https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/solution/liang-shu-zhi-he-ii-shu-ru-you-xu-shu-zu-by-leet-2/ "两数之和 II - 输入有序数组题解"

  [15aeb9c1]: https://leetcode-cn.com/problems/unique-binary-search-trees-ii/ "不同的二叉搜索树 II"
  [29d2b07c]: https://leetcode-cn.com/problems/unique-binary-search-trees-ii/solution/bu-tong-de-er-cha-sou-suo-shu-ii-by-leetcode-solut/ "不同的二叉搜索树 II题解"

  [d1c3ca0e]: https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/ "旋转数组的最小数字"
  [79c44736]: https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/solution/mian-shi-ti-11-xuan-zhuan-shu-zu-de-zui-xiao-shu-3/ "旋转数组的最小数字题解"

  [8d44eb25]: https://leetcode-cn.com/problems/minimum-path-sum/ "最小路径和"
  [839d5e7d]: https://leetcode-cn.com/problems/minimum-path-sum/solution/zui-xiao-lu-jing-he-by-leetcode-solution/ "最小路径和题解"

  [5850a1ff]: https://leetcode-cn.com/problems/divisor-game/ "除数博弈"
  [f6c1a0e1]: https://leetcode-cn.com/problems/divisor-game/solution/chu-shu-bo-yi-by-leetcode-solution/ "除数博弈题解"

  [3f190060]: https://leetcode-cn.com/problems/split-array-largest-sum/ "分割数组的最大值"
  [54d569a1]: https://leetcode-cn.com/problems/split-array-largest-sum/solution/fen-ge-shu-zu-de-zui-da-zhi-by-leetcode-solution/ "分割数组的最大值题解"

  [7976e98d]: https://leetcode-cn.com/problems/longest-increasing-path-in-a-matrix/ "矩阵中的最长递增路径"
  [80d272a7]: https://leetcode-cn.com/problems/longest-increasing-path-in-a-matrix/solution/ju-zhen-zhong-de-zui-chang-di-zeng-lu-jing-by-le-2/ "矩阵中的最长递增路径题解"

  [b5e5b225]: https://leetcode-cn.com/problems/is-subsequence/ "判断子序列"
  [d646d4c4]: https://leetcode-cn.com/problems/is-subsequence/solution/pan-duan-zi-xu-lie-by-leetcode-solution/ "判断子序列题解"

  [6f48a2d0]: https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/ "二叉树的最大深度"
  [1fd2273a]: https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/solution/er-cha-shu-de-zui-da-shen-du-by-leetcode-solution/ "二叉树的最大深度题解"

  [ef3b2276]: https://leetcode-cn.com/problems/xun-bao/ "寻宝"
  [5620f3ca]: https://leetcode-cn.com/problems/xun-bao/solution/xun-bao-bfs-dp-by-leetcode-solution/ "寻宝题解"

  [42b1ab82]: https://leetcode-cn.com/problems/integer-break/ "整数拆分"
  [760c099f]: https://leetcode-cn.com/problems/integer-break/solution/zheng-shu-chai-fen-by-leetcode-solution/ "整数拆分题解"

  [08950398]: https://leetcode-cn.com/problems/magic-index-lcci/ "魔术索引"
  [bdb57086]: https://leetcode-cn.com/submissions/detail/93428056/ "魔术索引题解"

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
