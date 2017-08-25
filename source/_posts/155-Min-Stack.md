---
title: 155. Min Stack
date: 2017-08-25 23:31:39
tags: LeetCode
categories: LeetCode
---

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

push(x) --Push element x onto stack.
pop() --Removes the element on top of the stack.
top() --Get the top element.
getMin() --Retrieve the minimum element in the stack.

Example:

MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   -- > Returns - 3.
minStack.pop();
minStack.top();      -- > Returns 0.
minStack.getMin();   -- > Returns - 2.

<!--more-->

## 题意：

　　设计一个支持入栈、出栈、获取栈顶元素，并在能在常数时间内取出的最小元素。

## 思路：

　　主要是设计常量时间获取栈顶最小素，主要是通过存储最小元素栈来实现，当push元素的时候首先插入常规栈，然后和最小元素栈的栈顶元素比较是否小于等于，如果是就入最小元素栈；当pop元素的时候比较普通栈顶和最小元素栈顶是否相同，如果相同，就同时弹出；当top获取栈顶元素的时候直接返回普通栈栈顶元素；getMin时直接返回最小栈的栈顶原素。

```c++
class MinStack {
public:
	void push(int x)
	{
		val.push(x);//先插入普通栈，如果小于等于最小元素栈栈顶元素，则插入最小元素栈
		if (minVal.empty() || x <= minVal.top())
			minVal.push(x);
	}
	void pop()
	{
		if (minVal.top() == val.top())//弹出时，只有最小元素栈栈顶和普通栈栈顶相等的时候最小元素栈才弹出栈顶元素
		{
			minVal.pop();
			val.pop();
		}
		else
			val.pop();
	}
	int top()
	{
		return val.top();
	}
	int getMin()
	{
		return minVal.top();
	}
private:
	stack<int> val;//普通栈
	stack<int> minVal;//最小元素栈
};
```

