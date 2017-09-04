---
title: 224. Basic Calculator
date: 2017-08-31 22:30:03
tags: LeetCode
categories: LeetCode
---

Implement a basic calculator to evaluate a simple expression string.
he expression string may contain open(and closing parentheses), the plus + or minus sign - , non - negative integers and empty spaces .

You may assume that the given expression is always valid.
Some examples :
"1 + 1" = 2
" 2-1 + 2 " = 3
"(1+(4+5+2)-3)+(6+8)" = 23

<!--more-->

## 题意：

　　实现基本的计算器来计算一个简单的表达式字符串，表达式字符串可能包含打开“（”和闭括号“）”，加号或减号，非负整数和空格。

## 思路：

### 　方法一：

　　两个要点：
　　　　1、无括号时，顺序执行
　　　　2、有括号时，先执行括号中的
　　两个栈：
　　　　一个存放操作数，每次进栈要注意，如果操作符栈顶元素为'+'/'-'，则需要立即计算。
　　　　一个存放操作符（包括括号），每次出现')'时，不断进行出栈计算再进栈，直到弹出'('，说明当前括号内计算完毕。

```c++
class Solution {
public:
	int calculate(string s) {
		stack<int> num;//存放操作数
		stack<int> op;//存放操作符
		int i = 0;
		while (i < s.size())
		{
			while (i < s.size() && s[i] == ' ')//跳过字符串中的空格字符
				i++;
			if (i == s.size())
				break;
			if (s[i] == '+' || s[i] == '-' || s[i] == '(')
			{
				op.push(s[i]);
				i++;
			}
			else if (s[i] == ')')
			{
				while (op.top() != '(')
				{// calculation within parentheses 
					int n2 = num.top();
					num.pop();
					int n1 = num.top();
					num.pop();
					if (op.top() == '+')
						num.push(n1 + n2);
					else
						num.push(n1 - n2);
					op.pop();
				}
				op.pop();
				while (!op.empty() && op.top() != '(')
				{
					int n2 = num.top();
					num.pop();
					int n1 = num.top();
					num.pop();
					if (op.top() == '+')
						num.push(n1 + n2);
					else
						num.push(n1 - n2);
					op.pop();
				}
				i++;
			}
			else
			{
				int n = 0;
				while (i < s.size() && s[i] >= '0' && s[i] <= '9')
				{
					n = n * 10 + (s[i] - '0');
					i++;
				}
				num.push(n);
				while (!op.empty() && op.top() != '(')
				{
					int n2 = num.top();
					num.pop();
					int n1 = num.top();
					num.pop();
					if (op.top() == '+')
						num.push(n1 + n2);
					else
						num.push(n1 - n2);
					op.pop();
				}
			}
		}
		return num.top();
	}
};
```

### 　方法二：

　　由于表达式中只含有括号和加减法运算，我们可以通过加减法的规律对表达式进行化简，，因为只有加减，去括号的话只会影响括号内部的符号变换，表达式都是从左到右依次执行的，然后求值。

```c++
class Solution {
public:
	int calculate(string s) {
		stack<int> num;
		num.push(1);
		char op = '+';
		long re = 0;
		for (int i = 0; i < s.length(); i++) {
			switch (s[i]) {
			case ' ':
				break;
			case '+':
			case '-':
				op = s[i];
				break;
			case '(':
				num.push(num.top() * (op == '-' ? -1 : 1));
				op = '+';
				break;
			case ')':
				num.pop();
				break;
			default:
				int tmp = 0;
				while (s[i] >= '0' && s[i] <= '9') {
					tmp = tmp * 10 + s[i] - '0';
					i++;
				}
				i--;
				re += (op == '-' ? -1 : 1)*num.top()*tmp;
			}
		}
		return re;
	}
};
```

### 　方法三：

　　通用性代码，可以包含乘除，加括号等。

```c++
class Solution {
public:
    int calculates5(string s) {
        if (s.empty())
        {
            return 0;
        }
        //1、把表达式去掉空格存到队列中
        queue<string> que;//存放表达式序列
        for (int i=0;i<s.size();)
        {
            string	str = "";
            switch (s[i])
            {
            case ' ':
                i++;
                break;
            case '+':
            case '-':
            case '(':
            case ')':
                str = s[i];
                que.push(str);
                i++;
                break;
            default:
                while (i<s.size()&&s[i]>='0'&&s[i]<='9')
                {
                    str += s[i];
                    i++;
                }
                que.push(str);
                break;
            }
        }
        //2、把中缀表达式转换为后缀表达式
        stack<string> stackA;
        stack<string> stackB;
        while (!que.empty())
        {
            string temp = que.front();
            que.pop();
            char op = temp.at(0);
            if (op=='(')
            {
                stackB.push(temp);
            }
            else if (op==')')
            {
                while (!stackB.empty() && stackB.top() != "(")
                {
                    stackA.push(stackB.top());
                    stackB.pop();
                }
                if (!stackB.empty())
                {
                    stackB.pop();
                }
            }
            else if (op=='+' || op =='-')
            {
                if (stackB.empty() || stackB.top()=="(")
                {
                    stackB.push(temp);
                }
                else {
                    while (!stackB.empty()&&stackB.top()!="(")
                    {
                        stackA.push(stackB.top());
                        stackB.pop();
                    }
                    stackB.push(temp);
                }
            }
            else
            {
                stackA.push(temp);
            }
        }
        while (!stackB.empty())
        {
            stackA.push(stackB.top());
            stackB.pop();
        }
        while (!stackA.empty())
        {
            stackB.push(stackA.top());
            stackA.pop();
        }
        //3、通过后缀表达式进行计算
        stack<int> result;
        while (!stackB.empty())
        {
            int res = 0;
            string temp = stackB.top();
            stackB.pop();
            char op = temp.at(0);
            switch (op)
            {
            case '+':
                res = result.top();
                result.pop();
                res += result.top();
                result.pop();
                result.push(res);
                break;
            case '-':
                res = result.top();
                result.pop();
                res = result.top() - res;
                result.pop();
                result.push(res);
                break;
            default:
                result.push(atoi(temp.c_str()));
                break;
            }
        }
        return result.top();
    }
};
```

