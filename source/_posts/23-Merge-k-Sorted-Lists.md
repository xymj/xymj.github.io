---
title: 23. Merge k Sorted Lists
date: 2017-07-08 19:03:17
tags: LeetCode
categories: LeetCode
---

# 23. Merge k Sorted Lists

Merge k sorted linked lists and return it as one sorted list.Analyze and describe its complexity.

## 题意：

　　合并K个排序的链表并将其返回为一个排序列表，并分析其复杂性。

<!--more-->

## 思路：

### 　方法一：

　　直接利用暴力解决的办法，首先利用两个有序链表的合并算法[21. Merge Two Sorted Lists](http://blog.taoaili999.cn/2017/07/06/21-Merge-Two-Sorted-Lists/)，然后依次遍历k个有序链表，返回合并后的排序链表，但是时间复杂度极高，效率很低。

```c++
//运行时间过高,You are here!   Your runtime beats 3.58% of cppsubmissions.
//两个有序链表合并
ListNode* mergeList(ListNode *l1, ListNode *l2) {
	ListNode newHead(-1);
	ListNode *p = &newHead;
	while (l1!=NULL&&l2!=NULL)
	{
		if (l1->val<l2->val)
		{
			p->next = l1;
			l1 = l1->next;
		}
		else
		{
			p->next = l2;
			l2 = l2->next;
		}
		p = p->next;
	}
	while (l1)
	{
		p->next = l1;
		l1 = l1->next;
		p = p->next;
	}
	while (l2)
	{
		p->next = l2;
		l2 = l2->next;
		p = p->next;
	}
	return newHead.next;
}
//遍历k个链表数组
ListNode* mergeKLists(vector<ListNode*>& lists) {
	int n = lists.size();
	if (n==0)
	{
		return NULL;
	}
	ListNode *newHead = NULL;
	for (int i = 0; i < n; i++)
	{
		 newHead = mergeList(lists[i],newHead);
	}
	return newHead;
}
```

### 　方法二：

　　利用堆排序的思想，将每个链表的表头元素取出来，建立一个小顶堆，因为k个链表中都排好序了，因此每次取堆顶的元素就是k个链表中的最小值，可以将其合并到合并链表中，再将这个元素的指针指向的下一个元素也加入到堆中，再调整堆，取出堆顶，合并链表。。。。以此类推，直到堆为空时，链表合并完毕。
　　 建堆的时间复杂度是k/2logk, 每次取出堆顶再加入元素的复杂度是logk,假设每条链表平均有n个元素，则一共有nk-k次。因此总的时间复杂度为O(nklogk)。


```c++
 // 使用堆排序,
	// 1. 选出每个链表的头来插入小顶堆中，
	// 2. 再把堆顶接入合并链表中，
	// 3. 被选出的指针后移再加入小顶堆中,回到2
	// 4. 最后所有链表都为空时，返回合并链表的头指针
 //You are here!  Your runtime beats 40.40% of cppsubmissions.
class Solution{
  //1、建立小顶堆
  void creatHeap(vector<ListNode*> &heap) {
      for (int i = heap.size()/2-1;i>=0;i--)
      {
          downElement(heap, i);
      }
  }
  //2、下沉元素：
        //（1）当传递的是最后一个非叶子节点的索引时（从最后一个非叶子节点开始，将每个父节点都调整为最小堆），是建立最小堆。
        //（2）当传递的是堆的第一个元素时（即完全二叉树的根节点向下沉，是进行堆排序输出最小节点后，继续生成最小堆），堆排序过程中生成最小堆。
  void downElement(vector<ListNode*> &heap,int elementIndex) {
      int min = 0;
      int index = elementIndex;
      while (2*index+1<heap.size())//存在左孩子节点
      {
          min = 2 * index + 1;
          if (2*index+2<heap.size())//存在右孩子节点
          {
              if (heap[2 * index + 2]->val<heap[min]->val)//确定最小孩子节点索引
              {
                  min = 2 * index + 2;
              }
          }
          if (heap[index]->val<heap[min]->val)//如果根节点小于最小孩子节点，此子树已经是最小堆，直接结束
          {
              break;
          }
          else
          {
              swap(heap[index], heap[min]);//上移最小孩子节点
              index = min;//孩子节点索引做为根节点继续遍历
          }
      }
  }
  //3、堆的首元素弹出
  ListNode* popHeap(vector<ListNode*> &heap) {
      auto temp = heap[0];
      swap(heap[0], heap[heap.size() - 1]);
      heap.pop_back();
      downElement(heap, 0);
      return temp;
  }
  //4、堆中添加元素
  void pushHeap(vector<ListNode*> &heap,ListNode *p) {
          heap.push_back(p);
          //1、主要这个地方重新建堆造成runtime低
          // creatHeap(heap);
          //2、效率也不是很高
          /*int child = heap.size();
          int parent = child / 2;
          while (parent)
          {
              if (heap[child-1]->val<heap[parent-1]->val)
              {
                  swap(heap[child-1], heap[parent-1]);
              }
              child = parent;
              parent = child / 2;
          }*/
          //3、效率相对高
          for (int child = heap.size(), parent = child / 2; parent; child--, parent = child / 2) {
              if (heap[child - 1]->val < heap[parent - 1]->val) //判断新加入的元素是否比根节点小，如果小要进行下沉、替换
              {
                  swap(heap[child - 1], heap[parent - 1]);
              }
          }
  }
  ListNode* mergeKLists(vector<ListNode*>& lists) {
      vector<ListNode*> heap;
      for (int i = 0; i != lists.size(); i++) {
          if (lists[i]) heap.push_back(lists[i]);
      }
      if (heap.empty())
      {
          return NULL;
      }
      ListNode newHead(-1);
      ListNode *p = &newHead;
      creatHeap(heap);
      while (!heap.empty())
      {
          ListNode *minHeap = popHeap(heap);
          p->next = minHeap;
          p = p->next;
          auto next = minHeap->next;
          if (next)
              pushHeap(heap,next);
          //creatHeap(heap);//这种重新再次构建最小堆会超时
      }
      return newHead.next;
  }
};

```
---------------------------------------------------
#### 　Java Code：
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
 //自己实现小顶堆，思想借鉴PriorityQueue源码
class Solution {
    private ListNode[] minHeap = null;
    private int size = 0;
    public ListNode mergeKLists(ListNode[] lists) {
        if (null == lists) {
            return null;
        }

        int len = lists.length;
        if (len == 0) {
            return null;
        }
        minHeap = new ListNode[len];
        for (int i = 0; i < len;i++) {
            if (null != lists[i]) {
                minHeap[size] = lists[i];
                size++;
            }   
        }
        if (size == 0) {
            return null;
        }
        heapify();
        ListNode head = new ListNode(-1);
        ListNode p = head;
        while (size > 0) {
            ListNode minNode = poll();
            p.next = minNode;
            minNode = minNode.next;
            p = p.next;
            p.next = null;
            if (null != minNode) {
                offer(minNode);
            }
        }
        return head.next;
    }

    public void heapify() {
        for (int i = (size >>> 1) - 1;i >= 0;i--) {
            siftDown(i, minHeap[i]);
        }
    }

    public void siftDown(int index, ListNode node) {
        int half = size >>> 1;
        while (index < half) {
            int min = (index << 1) + 1;
            ListNode child = minHeap[min];
            int right = min + 1;
            if (right < size && minHeap[right].val < child.val) {
                child = minHeap[min = right];
            }
            if (child.val >= node.val) {
                break;
            }
            minHeap[index] = child;
            index = min;
        }
        minHeap[index] = node;
    }

    public void siftUp(int index, ListNode node) {
        while (index > 0) {
            int parent = (index - 1) >>> 1;
            if (minHeap[parent].val <= node.val) {
                break;
            }
            minHeap[index] = minHeap[parent];
            index = parent;
        }
        minHeap[index] = node;
    }

    public ListNode poll() {
        if (size == 0) {
            return null;
        }

        ListNode minNode = minHeap[0];
        int last = --size;
        ListNode lastNode = minHeap[last];
        minHeap[last] = null;
        if (last != 0) {
            siftDown(0, lastNode);
        }
        return minNode;
    }

    public void offer(ListNode node) {
        if (size == 0) {
            minHeap[0] = node;
            size++;
            return;
        }
        minHeap[size] = node;
        siftUp(size, node);
        size++;
    }
}
```

```java
//直接使用Java自带的优先队列PriorityQueue
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (null == lists) {
            return null;
        }

        int len = lists.length;
        if (len == 0) {
            return null;
        }

        List<ListNode> minHeap = new ArrayList();
        for (int i = 0; i < len;i++) {
            if (null != lists[i]) {
                minHeap.add(lists[i]);
            }   
        }
        if (minHeap.size() == 0) {
            return null;
        }
        PriorityQueue<ListNode> queue = new PriorityQueue(minHeap.size(), new Comparator<ListNode>() {
            @Override
            public int compare(ListNode node1, ListNode node2) {
                if (node1.val > node2.val) {
                    return 1;
                } else if (node1.val < node2.val) {
                    return -1;
                } else {
                    return 0;
                }
            }
        });
        queue.addAll(minHeap);
        ListNode head = new ListNode(-1);
        ListNode p = head;
        while (queue.size() > 0) {
            ListNode minNode = queue.poll();
            p.next = minNode;
            minNode = minNode.next;
            p = p.next;
            p.next = null;
            if (null != minNode) {
                queue.offer(minNode);
            }
        }
        return head.next;
    }

}
```
