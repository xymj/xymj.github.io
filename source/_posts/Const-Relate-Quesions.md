---
title: C++  const 用法
date: 2017-08-13 10:49:02
tags: C++
categories: C++
---

# C++  const 用法

　　关于指针，主要注意四个方面：指针类型、指针指向类型、指针的值、指针指向的值。当const修饰的是指针类型，那么指针的值就不能改变，即不能指向其他内存空间，但是可以通过指针修改其指向内存空间里面的值。当const修饰是指针指向的内存空间时，那么指针可以指向其他内存空间，但是不能通过指针修改指针所指向内存空间里面的值。

　　C++ const 允许指定一个语义约束，编译器会强制实施这个约束，允许程序员告诉编译器某值是保持不变的。如果在编程中确实有某个值保持不变，就应该明确使用const，这样可以获得编译器的帮助。

<!--more-->

## **1.const 修饰成员变量** 

```c++
#include<iostream>
using namespace std;
int main(){
    int a1=3;   ///non-const data
    const int a2=a1;    ///const data

    int * a3 = &a1;   ///non-const data,non-const pointer
    const int * a4 = &a1;   ///const data,non-const pointer
    int * const a5 = &a1;   ///non-const data,const pointer
    int const * const a6 = &a1;   ///const data,const pointer
    const int * const a7 = &a1;   ///const data,const pointer
  
    return 0;
}
```

### **const修饰指针变量时：**

　　(1)只有一个const，如果const位于*左侧，表示指针所指数据（所指内存空间）是常量，不能通过解引用修改该数据；指针本身是变量，可以指向其他的内存单元。

　　(2)只有一个const，如果const位于*右侧，表示指针本身是常量，不能指向其他内存空间；指针所指的数据可以通过解引用修改。

　　(3)两个const，*左右各一个，表示指针和指针所指数据（所指内存地址数据）都不能修改。

## **2.const修饰函数参数**

　　传递过来的参数在函数内不可以改变，与上面修饰变量时的性质一样。

```c++
void testModifyConst(const int x) {
     x=5;　　　///编译出错
}
```

## **3.const修饰成员函数**

　　(1) const修饰的成员函数不能修改任何的成员变量(**mutable修饰的变量除外**) 
　　　mutable：在C++中，mutable是为了突破const的限制而设置的。被mutable修饰的变量，将永远处于可变的状态，即使在一个const函数中，甚至结构体变量或者类对象为const，其mutable成员也可以被修改。

```c++
struct  ST  
{  
  int a;
  mutable int b; 
};  
const ST st={1,2};  
st.a=11;//编译错误  
st.b=22;//允许  
```

　　(2) const成员函数不能调用非onst成员函数，因为非const成员函数可以会修改成员变量

```c++
#include <iostream>
using namespace std;
class Point{
    public :
    Point(int _x):x(_x){}

    void testConstFunction(int _x) const{
        ///错误，在const成员函数中，不能修改任何类成员变量
        x=_x;

        ///错误，const成员函数不能调用非onst成员函数，因为非const成员函数可以会修改成员变量
        modify_x(_x);
    }

    void modify_x(int _x){
        x=_x;
    }

    int x;
};
```

## **4.const修饰函数返回值**

(1)指针传递
　　如果返回const data,non-const pointer，返回值也必须赋给const data,non-const pointer。因为指针指向的数据(内存空间里面的值)是常量不能修改。

```c++
const int * mallocA(){  ///const data,non-const pointer
    int *a=new int(2);
    return a;
}

int main()
{
    const int *a = mallocA();
    ///int *b = mallocA();  ///编译错误
    return 0;
}
```

(2)值传递
　　如果函数返回值采用“值传递方式”，由于函数会把返回值复制到外部临时的存储单元中，加const 修饰没有任何价值。所以，**对于值传递来说，加const没有太多意义。**

所以：
　　不要把函数int GetInt(void) 写成const int GetInt(void)。
　　不要把函数A GetA(void) 写成const A GetA(void)，其中A 为用户自定义的数据类型。

　　**在编程中如果确定不变的常量值，要尽可能多的使用const，这样可以获得编译器的帮助，以便写出健壮性的代码。**