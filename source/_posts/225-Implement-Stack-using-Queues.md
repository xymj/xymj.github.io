---
title: 225. Implement Stack using Queues
date: 2017-08-31 23:17:29
tags: LeetCode
categories: LeetCode
---

Implement the following operations of a stack using queues.

push(x) --Push element x onto stack.
pop() --Removes the element on top of the stack.
top() --Get the top element.
empty() --Return whether the stack is empty.

Notes:
You must use only standard operations of a queue -- which means only push to back, peek / pop from front, size, and is empty operations are valid.
Depending on your language, queue may not be supported natively.You may simulate a queue by using a list or deque(double - ended queue), as long as you use only standard operations of a queue.
You may assume that all operations are valid(for example, no pop or top operations will be called on an empty stack).

<!--more-->

## 题意：

　　使用队列实现堆栈的push(x) ，pop()，top()，empty()操作。
　　只能使用一个队列的标准操作 - 这意味着只能从前面，后面，peek / pop从前面，大小，并且是空的操作是有效的。
根据您的语言，队列可能不被本机支持。您可以使用列表或deque（双端队列）来模拟队列，只要您仅使用队列的标准操作即可。可以假设所有操作都有效（例如，在空堆栈上不会调用pop或top操作）。

## 思路：

### 　方法一：

　　用两个队列模拟一个堆栈：

```c++
队列a和b
 （1）取栈顶元素： 返回有元素的队列的首元素
 （2）判栈空：若队列a和b均为空则栈空
 （3）入栈：a队列当前有元素，b为空（倒过来也一样）则将需要入栈的元素先放b中，然后将a中的元素依次出队列并入列倒b中。（保证有一个队列是空的）
 （4）出栈：将有元素的队列出列即可。
比如先将1插入队a中 ，现在要将2入栈，则将2插入b中然后将a中的1出列入到b中，b中的元素变为 2 ，1
a为空，现在要压入3 则将3插入a中 ，依次将b中的2 ，1 出列并加入倒a中 ，a中的元素变为 3，2，1 b为空
注意：算法保证在任何时候都有一队列为空
```

```c++
class Stack {
	queue<int> q1;
	queue<int> q2;
public:
	// Push element x onto stack.
	void push(int x) {
		if (q1.empty())
		{
			q1.push(x);
			while (!q2.empty())
			{
				int temp = q2.front();
				q2.pop();
				q1.push(temp);
			}
		}
		else
		{
			q2.push(x);
			while (!q1.empty())
			{
				int temp = q1.front();
				q1.pop();
				q2.push(temp);
			}
		}
	}

	// Removes the element on top of the stack.
	void pop() {
		if (!q1.empty())
			q1.pop();
		if (!q2.empty())
			q2.pop();
	}

	// Get the top element.
	int top() {
		if (!q1.empty())
			return q1.front();
		if (!q2.empty())
			return	q2.front();
	}

	// Return whether the stack is empty.
	bool empty() {
		return	q1.empty()&&q2.empty();
	}
};
```

### 　方法二：

　　 利用双端队列，实现代码简单，但是不懂原理，效率低，真正要学习的是两个队列实现栈，以及两个栈实现队列。

```c++
class Stack {
	deque<int> res;
public:
	// Push element x onto stack.
	void push(int x) {
		res.push_back(x);
	}

	// Removes the element on top of the stack.
	void pop() {
		res.pop_back();
	}

	// Get the top element.
	int top() {
		return res.back();
	}

	// Return whether the stack is empty.
	bool empty() {
		return	res.empty();
	}
};
```

