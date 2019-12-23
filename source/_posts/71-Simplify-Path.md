---
title: 71. Simplify Path
date: 2017-07-31 22:31:03
tags: LeetCode
categories: LeetCode
---

# 71. Simplify Path

Given an absolute path for a file(Unix - style), simplify it.

For example,
path = "/home/", = > "/home"
path = "/a/./b/../../c/", = > "/c"

<!--more-->

## 题意：

　　给定一个文件的绝对路径（UNIX样式），简化它，也就是通过一系列转换后，最后所在目录的路径。

## 思路：

　　这道题的要求是简化一个Unix风格下的文件的绝对路径。
　　字符串处理，由于".."是返回上级目录（如果是根目录则不处理），因此可以考虑用栈记录路径名，以便于处理。需要注意几个细节：
　　重复连续出现的'/'，只按1个处理，即跳过重复连续出现的'/'；
　　如果路径名是"."，则不处理；
　　如果路径名是".."，则需要弹栈，如果栈为空，则不做处理；
　　如果路径名为其他字符串，入栈。
　　最后，再逐个取出栈中元素（即已保存的路径名），用'/'分隔并连接起来，不过要注意顺序。

```c++
//此法是通过string的find_first_of方法查找'/'字符位置，然后截取字符串，再判断字符串的形式，是否入栈，或者出栈，或者不做任何处理
class Solution {
public:
	string simplifyPath(string path) {
		stack<string> s;
		string res = "";
		size_t  pos1 = path.find_first_of('/');
		while (pos1!=string ::npos)
		{
			size_t temp = pos1;
			pos1 = path.find_first_of('/',pos1+1);
			string tempString = "";
			if (pos1 == string :: npos)
			{
				 tempString = path.substr(temp+1);
			}
			else
				 tempString = path.substr(temp+1,pos1-temp-1);
			if (tempString == "..")
			{
				if(!s.empty())
					s.pop();
				else
					continue;
			}
			else if(tempString == ".")
			{
				continue;
			}
			else if(tempString != "")
			{
				s.push(tempString);
			}
		}
		if (s.empty())
		{
			res = '/';
		}
		while (!s.empty())
		{
			string val = s.top();
			res = '/' + val + res;
			s.pop();
		}
		return res;
	}
};
```

```c++
//此法是通过string的下标索引，逐个遍历每个字符查找'/'字符位置，然后根据索引直接累加字符到下一个'/'字符处，得到目录字符串，再判断字符串的形式，是否入栈，或者出栈，或者不做任何处理
class Solution {
public:
	string simplifyPath(string path)
	{
		stack<string> ss; // 记录路径名
		for (int i = 0; i < path.size(); )
		{
			// 跳过斜线'/'
			while (i < path.size() && '/' == path[i])
				++i;
			// 记录路径名
			string s = "";
			while (i < path.size() && path[i] != '/')
				s += path[i++];
			// 如果是".."则需要弹栈，否则入栈
			if (".." == s && !ss.empty())
				ss.pop();
			else if (s != "" && s != "." && s != "..")
				ss.push(s);
		}
		// 如果栈为空，说明为根目录，只有斜线'/'
		if (ss.empty())
			return "/";
		// 逐个连接栈里的路径名
		string s = "";
		while (!ss.empty())
		{
			s = "/" + ss.top() + s;
			ss.pop();
		}
		return s;
	}
};
```

---------------------------------------------------
### 　Java Code
```Java
class Solution {
    public String simplifyPath(String path) {
        if (null == path) {
            return "";
        }

        int length = path.length();
        if (length == 0) {
            return "";
        }

        Stack<String> stack = new Stack();
        int i = 0;
        while (i < length) {
            while (i < length && path.charAt(i) == '/') {
                i++;
            }

            StringBuilder tmpDir = new StringBuilder();
            while (i < length && path.charAt(i) != '/') {
                tmpDir.append(path.charAt(i));
                i++;
            }

            if ("..".equals(tmpDir.toString())) {
                if (!stack.empty()) {
                    stack.pop();
                }
                continue;
            }

            if (".".equals(tmpDir.toString()) || "".equals(tmpDir.toString())) {
                continue;
            }

            stack.push(tmpDir.toString());
        }

        if (stack.empty()) {
            return "/";
        }

        ArrayList<String> dirs = new ArrayList();
        dirs.addAll(stack);
        StringBuilder result = new StringBuilder();
        dirs.forEach(dir -> {
            result.append("/").append(dir);
        });


        return result.toString();
    }
}
```
