---
title: 150. Evaluate Reverse Polish Notation
date: 2017-08-24 23:00:00
tags: LeetCode
categories: LeetCode
---

Evaluate the value of an arithmetic expression in Reverse Polish Notation.

Valid operators are + , -, *, / .Each operand may be an integer or another expression.

Some examples :
["2", "1", "+", "3", "*"] -> ((2 + 1) * 3) -> 9
["4", "13", "5", "/", "+"] -> (4 + (13 / 5)) -> 6

<!--more-->

## 题意：

　　把逆波兰表达式（后缀表达式）转换为正常的算术运算表达式，然后对算术运算表达式进行求值。
　　有效的运算符+，-，*，/，每个操作数可以是整数或一个表达式。

## 思路：

　　主要利用容器栈进行表达式的转换 。从左到右读表达式，如果读到[操作数](https://baike.baidu.com/item/%E6%93%8D%E4%BD%9C%E6%95%B0)就将它压入栈S中，如果读到n元运算符(即需要参数个数为n的运算符)则取出由栈顶向下的n项按操作数运算，再将运算的结果代替原栈顶的n项，压入栈S中 。如果后缀表达式未读完，则重复上面过程，最后输出栈顶的数值则为结束。
　　其实这就是计算表达式求值的最后一步，当想要计算一个带括号，以及复杂运算符的表达式时，要先把中缀表达式，也就是普通算术运算表达式转换为后缀表达式，然后从前往后遍历后缀表达式，是值的话直接入栈，非值去栈顶元素，运算结束再插入栈顶，知道遍历后缀表达结束，栈顶元素就是普通表达式的值。

```c++
int evalRPN(vector<string>& tokens) {
	stack<int> stack;//辅助栈
	int size = tokens.size();
	for (int i = 0; i < size; i++) {
		string str = tokens[i];
		int first;//记录运算符对应的左操作数
		int second;//记录运算符对应的右操作数

		if (str == "+") {
			second = stack.top(); 
          	stack.pop();
			first = stack.top(); 
          	stack.pop();
			stack.push(second + first);
		}
		else if (str == "-") {
			second = stack.top(); stack.pop();
			first = stack.top(); stack.pop();
			stack.push(first - second);
		}
		else if (str == "*") {
			second = stack.top(); stack.pop();
			first = stack.top(); stack.pop();
			stack.push(first*second);
		}
		else if (str == "/") {
			second = stack.top(); stack.pop();
			first = stack.top(); stack.pop();
			stack.push(first / second);
		}
		else {
			//非运算符，为整数，直接解析并压栈
			int val = atoi(str.c_str());
			stack.push(val);
		}
	}
	return stack.top();
}
```

