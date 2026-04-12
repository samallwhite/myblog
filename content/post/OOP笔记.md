---
title: "OOP Notes"
date: 2026-04-12T21:36:44+08:00
draft: false
slug: "oop-notes"
tags:
  - "OOP"
  - "C++"
categories:
  - "Notes"
---

# 2.Making & Using Objects

## Declaration vs Definition

声明指出名字，定义分配空间

声明可以执行多次，但定义只能执行一次；

使用`extern`表示该语句仅仅是声明，而定义在随后/外部文件：

```cpp
extern int x;
extern int ADD(int a, int b);
```

```cpp
//: C02: Declare.cpp
// Declaration & definition examples
extern int i; // A
extern double f(double); // B
float b = 0.0; // C
double f(double a) { return a + 1.0; } // D
int i; // E
int h(int x) {return x + 1; } // F
void main( ) {
    b = 1.2;
    i = 2;
    f(b);
    h(i);
}
```

我们此处的`int i;//E`是定义&声明，所以我们如果需要在某处指定是声明，必须用`extern`以防止在多文件中重复定义。

## Linking

1. 首先是preprocessor，preprocessor把include的头文件复制进来，把宏定义替换；
2. 其次是compiler，其一是parsing：把代码变成语法树；其二是synthesizing：把语法书变成汇编/机器代码，并得到一个`.o/.obj`文件；
3. 第三步是Linker，他把所有`.o`连接起来，并寻找各引用位置；

## Basic Type

### String

```cpp
#include<iostream>
#include<string>
using namespace std;

int main(){
    string s1,s2;
    string s3 = "Hello World.";
    string s4("I am");
    s2 = "Today";
    s1 = s3 + " " + s4;
    s1 += "8";
    cout<<s1 + s2 + "!"<<endl;
    return 0;
}
```

### Stream

```cpp
#include<iostream>
#include<string>
#include<fstream>
using namespace std;
int main(){
    ifstream in("file1.cpp");
    ofstream in("file2.cpp");
    string s;
    while(getline(in,s)){
        out<<s<<"\n";
        cout<<s<<"\n";
    }
    return 0;
}
```

### Vector

Vector 是一个template

```cpp
#include<string>
#include<iostream>
#include<fstream>
#include<vector>
using namespace std;
int main(){
    vector<string> v;
    ifstream in("file1.cpp");
    string line;
    while(getline(in, line)){
        v.push_back(line);
    }
    for(int i=0;i<v.size();i++){
        cout<<i<<":"<<v[i]<<endl;
    }
    return 0;
}
```

# 3.The C in C++

## Introduction to pointers

cpp允许空指针`void*`的存在，但不允许给有类型指针赋空指针值：

```cpp
int i = 10;
int* p = &i;
void* vp = p; // OK
int* ip = vp; // error
```

## Argument Passing

|                      | Passing by value                                             | Passing by address                                     | Passing by reference                                   |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------------------ | ------------------------------------------------------ |
| **Formal arguments** | Variables                                                    | Pointers                                               | References                                             |
| **Actual arguments** | Data value                                                   | Address value                                          | Variable names                                         |
| **Characteristic**   | Data values of actual arguments are passed to formal arguments | Formal arguments are pointed to actual arguments       | Formal arguments refer to actual arguments             |
| **Effects**          | Formal and actual arguments are individual                   | Formal and actual arguments are changed simultaneously | Formal and actual arguments are changed simultaneously |

## Scoping



## Specifying storage allocation

- Static storage area:在程序运行前就被分配
- Stack:CPU直接用来存储运行数据(microprocessor)
- Heap:动态内存分配

### Global

global默认是可external linkage的，也就是说默认`int globe;` == `extern int globe;`

这可以保证多个文件中的globe都指向同一个地址

```cpp
```

### Local

在stack

no linkage

### Static

static只初始化一次，而且生存周期是整个程序

```cpp
#include<iostream>
using namespace std;
void func(){
	static int i=0;
    cout<<++i<<",";
}
int main(){
    for(int x=0;x<5;i++){
        func();
    }
    return 0;
}
//1,2,3,4,5
//if there's no static, the output should be 1,1,1,1,1
```

static 变量不对外暴露，无法跨文件访问

### Extern



### Constant&Volatile

Volatile:强制编译器每次从内存中重新读取变量（这可以预防变量被外部力量修改）

## Bitwise operators



## Shift operators

left-shif `<<`

right-shift`>>`

## Comma operator

```cpp
int a=0,b=1,c=2,d=3,e=4;
a = (b,c,d,e);
//a=4
a=b,c,d,e;
//a=1
b=(a=2*3,a*5);
//a=6,b=30
```

**逗号运算符会依次执行多个表达式，但最终结果是“最后一个表达式的值”**

第二个例子，因为逗号的优先级很低，所以事实是：`(a=b),c,d,e;`

## Casting operator

强制类型转换：

```cpp
int a = 100;
float b = float(a);
float c = (float)a;
int d = int(5.5);
```

## Function address

写成`void (*fp)();`

赋值的时候函数名会自动退化成函数指针；

函数指针的类型与函数返回类型一致

```cpp
//: C03:PointerToFunction.cpp
// Defining and using a pointer to a function
#include <iostream>
using namespace std;
void func(){ cout << "func() called..." << endl;}
int main() {
    void (*fp)( );
    fp = func;
    (*fp)( ); 
    // Call the function, func();
    void (*fp2)( ) = func;
    (*fp2)( ); // Call the function
    return 0;
}
```

## Dynamic Memory Allocation

使用`new`  `delete`分配和销毁空间



# 4.Data Abstraction

## The Class

```cpp
class Date{
private:
    int year,month,day;
public:
    void Set Date(int y,int m,int d);
    void Print(){
		cout<<year<<"."<<month<<"."<<day<<endl;
    }
};
void Date::SetDate(int y,int m,int d){
    year=y;
    month=m;
    day=d;
}
```

class的默认状态是private

若要访问类的方法：

```cpp
class Stash{};

int main(){
    Stash intStash;
    intStash.initialize(sizeof(int));
    ...
}
```

### this pointer

```cpp
void Stash::initialize(int sz){
    size=sz;//this->size = sz;
    quantity = 0;
}
//or to write
void Stash::initialize(int size){
    this->size = size;
    quantity = 0;
}
```

非静态成员函数的首个参数都是隐含的this指针，在这个例子里是`Stash*`类型

## Header file etiquette

```cpp
//: C04:Stack.h
// Nested struct in linked list
#ifndef STACK_H
#define STACK_H
class Stack {
    class Link {
        void* data;
        Link* next;
        void initialize(void* dat, Link* nxt);
    }* head;
    void initialize();
    void push(void* dat);
    void cleanup();
};
#endif // STACK_H

//: C04:Stack.cpp {O}
// Linked list with nesting
#include "Stack.h"
#include "../require.h"
using namespace std;
void Stack::Link::initialize(void* dat, Link* nxt)
{// To assign the arguments to the members.
    data = dat;
    next = nxt;
}
void Stack::initialize() { 
    head = 0; 
}
..........
```

## Global scope resolution

```cpp
int a;
void f(){};

class S{
public:
	int a;
    void f();
};
void S::f(){
    ::f();
    ::a++;//global a
    a--;//class a
}
int main(){
    S s;
    f();
    return 0;
}
```

# 5.Hiding the Implementaion

## Friend

普通的成员函数包含三点：访问private；属于类作用域；有this指针

而友元函数只有第一点。

• Asymmetric

• non-transitive

• cannot be inherited

```cpp
class A{
private:
    int x;
public:
    friend void show(A a);
};

void show(A a){
    a.x = 10;
}
```

### 友元作为global function：

```cpp
#include<iostream>
using namespace std;
class Time{
private:
    int hours,minutes;
public:
    void set(int nhours,int nminutes)
    {hours = nhours; minutes = nminutes;}
    friend void show(Time &time);
};
void show(Time& time){
    cout<<time.hours<<":"<<time.minutes<<endl;
}
int main(){
    Time time;
    time.set(20,30);
    show(time);
    return 0;
}
```

以上例子是global function as a friend；我们也有global function as friends，意味着一个函数可以是多个类的友元。

```cpp
#include<iostream>
using namespace std;
class Boat;
class Car{
private:
    int weight;
public:
    void set(int i)
};
class Boat{
private:
    int weight;
public:
    void set(int){weight = i};
    friend int totalWeight(Car &c,Boat &b);
};
int totalWeight(Car &c,Boat &b){
    return c.weight+b.weight;
}
```

### a member function as a friend

```cpp
#include<iostream>
#include<string>
using namespace std;
class Girl;
class Boy{
private:
    string name;
public:
    void init(string n){name=n};
    void Disp(Girl&);
};
class Girl {
public:
    void init(string n) { name = n; }
    friend void Boy::Disp(Girl&);
    private:
    string name;
};
void Boy::Disp(Girl& x) {
    cout << "Boy’s name is " << name << endl
        << "Girl’s name is " << x.name << endl;
}
```

### a class as a friend

在下面的例子中，就是Y中的所有方法都可以访问X的private成员

```cpp
#include<iostream>
using namespace std;

class X{
public:
    void Set(int i){x = i;}
    void Display(){
        cout<<"x"=<<x<<","<<"y="<<y<<endl;
    }
private:
    int x;
    static int y;
    friend class Y;
};

int X::y = 1;

class Y{
public:
    void Set(int i,int j);
    void Display();
private:
    X a;
};
void Y::Set(int i,int j){
    a.x = i;
    X::y = j;Y
}
void Y::Display(){
    cout<<"x="<<a.x<<",";
    cout<<"y="<<X::y<<endl;
}
```

关于静态成员的访问：

```cpp
class Test{
public:
    static int count;
};
//调用：
int Test::count = 0;
```

## Partial hiding & Handle class

```cpp
#ifndef HANDLE_H
#define HANDLE_H
class Handle{
    int i;
public:
    void initialize();
    void cleanup();
    ......
};
#endif
```

==to be continued==

# 6.Initialization & Cleanup

## Initialization with the constructor

a constructor has no return

```cpp
class X{
    int i;
public:
    X(){i=0;}
};
```

- constructor的第一个隐含参数是this指针；

- constructor可以有参数；

- constructor可以被overloaded；

```cpp
#include<iostream>
using namespace std;
class Date{
    int year,month,day;
public:
    Date(int y,int m,int d);
    void Print()
    {cout << year <<". "<< month <<". "<< day << endl;}
};

Date::Date(int y,int m,int d){
    year = y;
    month = m;
    day = d;
}
```

我们也可以有多个constructor

```cpp
class Student{
    int ID;
public:
    Student(int id){ID=id;}
    Student(int year,int order){
        ID = year*10000 + order;
    }
    ......
};
int main(){
    Student s1(20020001),s2(2002,1);
    return 0;
}
```

下面的是default constructor

```cpp
A(){}
```

## Cleanup with the destructor

• A destructor has not formal arguments andcannot be overloaded.

• A destructor has no return.

```cpp
class Date {
    int year, month, day;
public:
    Date(int y, int m, int d) { //constructor
        year = y; month = m; day = d;
        cout << "Constructor called. " << endl;
    }
    ~Date() {
        cout << "Destructor called. " << day << '.' << endl;
    }//destructor
    void Print() {
        cout << year << '.' << month << '.' << day << '.' << endl;
    }
};
```

![image-20260410203133108](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260410203133108.png)

## Constructor & Destructor

```cpp
#include<iostream>
using namespace std;
class Tree{
    int height;
public:
    Tree(int initialHeight);
    ~Tree();
    void grow(int years);
    void printtsize();
};
Tree::Tree(int initialHeight){
    height = initialHeight;
}
Tree::~Tree(){
    cout<<"inside Tree destructor"<<endl;
    printsize();
}
void Tree::grow(int years){
    height +=years;
}
void Tree::printsize(){
    cout<<"Tree height is "<<height<<endl;
}
```

## Initialization of Object Arrays

### Initialization of Arrays

```cpp
int a[5] = { 1, 2, 3, 4, 5 };
int b[6] = {2};
int c[] = { 1, 2, 3, 4 };
Size of an array=sizeof(c)/sizeof(c[0]);
```

可以通过一下方式初始化object arrays

```cpp
Date y[2] = {Date(22001,2,3),Date(2003,4,5)};

Date y[3];
y[0]=Date(2001,2,3);
y[1]=Date(2003,4,5);
y[2]=Date(2003,4,5);
```

```cpp
#include<iostream>
using namespace std;
class Date{
    int year,month,day;
public:
    Date(){
        year=mont=day=0;
        cout<<"Default constructor called."<<endl;
    }
    Date(int y,int m,int d){
        year =y;
        month = m;
        day = d;
        cout<<"Constructor called."<<day<<endl;
    }
    ~Date(){cout<<"Destructor called."<<day<<endl;}
    void Print(){cout<<year<<":"<<month<<":"<<day<<endl;}
};
int main(){
    Date dates[3] = {Date(2003,9,20),Date(2003,9,21)};
    for(int i=0;i<3;i++)
        dates[i].Print();
    return 0;
}
```

![image-20260410204118271](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260410204118271.png)

# 7.Function Overloading & Default Arguments

当有些函数在不同的类上执行相同的任务时，我们倾向于给这些函数相同的名字

![image-20260410204437378](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260410204437378.png)

这个过程就是overloading

编译器会按照参数的类型&数量选择最合适的函数，这个过程并不会考虑到参数的名称&返回类型

![image-20260410204850488](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260410204850488.png)

并且编译器还会考虑到built-in type cast，看看转换后的数据是否满足函数要求

## Default Arguments

```cpp
void print(int day=1){cout<<day<<'#';}
int main(){
    cout<<"Day";
    print();
    return 0;
}
```

默认参数必须放在最后

我们只能写如下表达：

```cpp
void fun(int a=1,int b=3,int c=5);
......
void fun(int a, int b, int c){
    ......
}
//而不能在第二次定义函数的时候再写一遍默认参数，不然会redefinition error
```

- **默认参数是静态绑定的：** 编译器在编译某个 `.cpp` 文件时，只看该文件中可见的函数声明来决定默认参数填什么。

![image-20260410212346862](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260410212346862.png)

![image-20260410212443380](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260410212443380.png)

# 8.Constants

## Pointers

### CONSTANT&POINTER

```CPP
const 在指针左边表示指针指向的内容是常量
const 在*右边表示指针本身是常量
```

### Pointer to const

```cpp
const int* u; int const* u;

int v1=3;
const int max = 100;
const int* p1=&v1;
*p1=5//error
p1 = &max;
*p1 = 10;//error
int *p2=&max;//error
```

以上代码总计三个错误：

- 第一、二个是因为当我们定义指向常量的指针，那么我们不能通过解引用方式修改指向的值；
- 第三个是因为我们不能用不是指向常量的指针指向常量，==但是常量指针可以指向非常量==，理解为指向常量的指针只是加入了只读限制。

### Const Pointer

```cpp
int d = 1; int* const w = &d;
int v1 = 10, v2 = 20;
const int max = 100;
int* const pv1 = &v1;
pv1 = &v2;//error
*pv1 = v2;
int* const pv2 = &max;//error
```

- 第一个错误是由于当我们定义了一个常量指针，那么我们就再也无法修改他的指向
- 第二个错误是由于我们虽然定义`pv2`为常量指针，但是其指向的量仍然可以通过该常量指针修改，那么`max`的常量性就无法得到维护。

### Constant Pointer to a const

```cpp
int v1 = 10, v2 = 20;
const int max = 100;
const int* const pv1 = &v1;
const int* const pv2 = &max;
pv1 = &v2;//error
*pv1 = v2;//error
```

- 第一个错误显而易见
- 第二个错误在于不能通过指向常量的指针修改变量

## Temporaries

我们在上文中给Date类赋值的时候有一句：

`Date date[3] = {Date(2003,4,5),Date(2003,4,5)};`

这句中的`Date(2003,4,5)`就会构造临时对象，这个临时对象会被编译器视为常量

## Classes

### Static data members

当你在类内定义了一个静态变量成员，那么这个类的所有实体都共用这一个变量，不会创造新的

![image-20260410224655150](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260410224655150.png)

类静态变量的正确定义方式：

只能在类外初始化

类外不需要static

类域名

```cpp
class Myclass{
    static int obj;
};
int Myclass::obj = 8;
```

const数据成员必须使用初始化列表初始化

而static const 应该单独处理

而且初始化顺序看的是在类内定义顺序，而非列表顺序

```cpp
class A{
public:
    A(int i);
    void Print();
private:
    const int a;
    static const int b;
    const int r;
};
const int A::b = 10;
A::A(int i):r(i),a(r+1);//正如刚才所说，于是这个赋值不能成功
```

### Const member functions

The const member functions do not modify the data members.

```cpp
class Date{
public:
    Date(int i, int j, int k){ y = i; j = m; d = k;}
    int year( ) const;
    int month( ) const { return m; }
    int day( ) { return d;}
private:
    int y, m, d ;
};
```

### Const objects

类的实体的生存期内没有数据成员被改变

```cpp
int main( ) {
    Date A(2003, 10, 1); // non-const object
    const Date B(2000, 9, 8); //const object
    return 0;
}
```

**常量对象（Const Objects）只能调用“承诺不修改数据”的方法（Const Member Functions）。**

### Mutable

mutable是常量对象的豁免符

```cpp
class Date {
public:
    // ... 构造函数 ...
    int year() const { return ++y; } // 注意这里：const 函数
private:
    mutable int y; // 注意这里：mutable 修饰
    int m, d;
};
```

# 9.Inline Functions

宏在替换的时候会有各种各样的问题

我们引入inline function，通过inline function可以使得经常被调用的函数减少运行时间

```cpp
inline int add(int x,int y,int z){
    return x+y+z;
}
int main(){
    int a(1),b(2),c(3)sum(0);
    sum=add(a,b,c);
}
```

（1）必须在调用前定义

（2）不要有循环 / switch

（3）不要有异常处理

（4）不要递归

## Class & Inline

类内实现的函数自动就是inline

如果函数在类外实现，那么需要加上inline来表明他是inline

```cpp
#include<iostream>
#include<string>
using namespace std;
class Point{
    int i,j,k;
public:
    Point();
    Point(int ii,int jj,int kk);
    void print(const string& msg="")const;
};
inline Point::Point():i(0),j(0),k(0){}
inline Point::Point(int ii,int jj,int kk):i(ii),j(jj),k(kk){}
inline void Point::print(const string& msg)cosnt{
    if(msg.size()!=0)cout<<msg;
    cout<<i<<","<<j<<","<<k<<endl;
}
```

==inline应该和类的定义在同一个文件中==

inline 函数必须在“每个使用它的翻译单元（.cpp）中都能看到定义

# 10.Name Control

static内置型变量被初始化成0，但是static user defined types会被constructor定义

![image-20260411110220662](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260411110220662.png)

# Controling linkage

Visibility:external linkage, internal linkage 

- External linkage: the names in file scope are visible for all translation units (.cpp files) in a program. **Global variables and ordinary functions have external linkage.** 
- Internal linkage (file static): the names are only visible for its translation unit. **Static, const and inline names default to internal linkage.**

## Namespace

| #    | namespace                                   | class/struct                      |
| ---- | ------------------------------------------- | --------------------------------- |
| 1    | **只能全局出现**，不能写在函数体里          | 可以在函数体里定义局部类          |
| 2    | 结束 **不用分号**                           | 结束必须写分号                    |
| 3    | **同名 namespace 可拆分** 到多个文件/头文件 | 同名 class 只能定义一次           |
| 4    | 可以 **起别名**`namespace ALib = MyLib;`    | 起别名要用 `using A = SomeClass;` |
| 5    | **不能实例化**                              | class 可以构造对象                |

```cpp
// 文件 a.h
namespace MyLib {
    void f();            // 只写声明
}

// 文件 a.cpp
#include "a.h"
namespace MyLib {        // 同名、同级别，自动合并
    void f() { ... }     // 写定义
}
```

```cpp
//: ScopeResolution.cpp
namespace X 
{
  	class Y 
	{
    	static int i;
  	public:
    	void f();
  	};
  	class Z;
  	void func();
}
int X::Y::i = 9;
void X::Y::f() { };
class X::Z {
  	int u, v, w;
public:
  	Z(int i);
  	int g();
}; 
X::Z::Z(int i) { u = v = w = i; }
int X::Z::g(){ return u=v=w=0;}
void X::func() {
  	X::Z  a(1); // object
  	a.g();
}
void  main(){ } ///:~
```

我们还可以只使用命名空间中的部分函数导入

```cpp
namespace calculator 
{
	double Add(double x, double y) { return x + y; }
	void Print(double x) { cout << "Result is " << x << endl; }
}
void main()
{	
	using calculator::Add; // Using Declaration
	double a, b;
	cin >> a >> b;
	calculator::Print(Add(a, b));
}
```

# 11.References & the Copy-Constructor

返回引用：

```cpp
int& g(int &x){
    x++;
    return x;
}
```

返回引用的好处是我们可以链式操作`g(g(a))`，并：在这个`g(a)`中，a被自动绑定了他的引用x

## Const Reference

既可以绑定普通变量（`g(a)` 正常运行），也可以绑定临时值 / 字面量（`g(1)` 正常运行）。

因为编译器会为临时值创建一个 “临时对象”，让常引用绑定到这个临时对象上（普通引用不允许这么做，只有常引用可以）。

```cpp
void f(int& i){i++;}
void g(const int& j){cout<<j;}

void main(){
	int a=1;
    f(1);//error;int &i=1;
    f(a);
    g(1);
    g(2);
}
```

## Pointer Reference

```cpp
//: C11:ReferenceToPointer.cpp
#include <iostream>
using namespace std;
void increment(int*& i) { i++; }
void main()
{
    int a = 2;
    int* p = &a;
    cout << "p = " << p << ": "<< *p <<endl;
    increment(p);
    cout << "p = " << p << ": "<< *p << endl;
} ///:~
```

我们在这个过程中直接修改了指针指向的内存

## The copy-constructor

写法：`X::X(const X& .)`

```cpp
#include <iostream>
using namespace std;
class Date{
	int year, month, day;
public:
    Date(int y=0, int m=0, int d=0){
        year=y; month=m; day=d;
    	cout<<"Constructor called."<<endl; 
    }
    Date(const Date & r){
        year=r.year; month=r.month; day=r.day;
        cout<<“Copy constructor called."<<endl;
    }
    ~Date( ) { cout<<"Destructor called."<<endl; }
};
void main() {
    Date d1(2003,9,20);
    Date d2=d1;
}
```

当令`d2=d1`构造的时候，调用拷贝构造


The Copy Constructor is called when

- A creating class object is initialized by another created object of the same class.
- passing arguments by values.
- a function returning an object value.

The Copy Constructor **is not called whenpassing arguments by references**. Because no new object is created.

![image-20260411170745301](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260411170745301.png)、

## Pointers to members

```cpp
//Pointers to data members
#include<iostream>
using namespace std;
class Data{
public:
    int a,b,c;
    void print() const{
        cout<<a<<","<<b<<","<<c<<endl;
    }
};

int main(){
    Data d;
    Data *dp = &d;
    int Data::*pmInt = &Data::a;
    dp->*pmInt = 47;
    pmInt = &Data::b;
    d.*pmInt = 48
}
```

调用方式：

```cpp
对象.*指针名
对象指针->*指针名
```

```cpp
class Widget {
public:
    void f(int) const { cout << "Widget::f()\n"; }
    void h(int) const { cout << "Widget::h()\n"; }
};

int main() {
    Widget w;
    Widget* wp = &w;

    // 定义一个指向Widget类中成员函数的指针pmem
    // 函数签名：返回void，参数int，const修饰
    // 让它指向Widget::h函数
    void (Widget::*pmem)(int) const = &Widget::h;

    // 通过对象w调用
    (w.*pmem)(1);  // 输出：Widget::h()

    // 通过对象指针wp调用
    (wp->*pmem)(2); // 输出：Widget::h()
    return 0;
}
```

# 12.Operator Overloading

```cpp
type operator @(argument list)
{
    ...
}
```

```cpp
#include<iostream>
using namespace std;
class Integer{
    int i;
public:
    Integer(int ii)::i(ii){}
    void print(){cout<<i<<endl;}
    const Integer operator+(const Integer& rv)const;
};
const Integer Integer::operator+(const Integer& rv)const{
    return Integer(i+rv.i);
}
```

这个operator的调用事实上是`Integer c = a+b;//a.operator+(b)`

使用引用类型可以避免过程中产生的拷贝

![image-20260411173058700](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260411173058700.png)

```cpp
class complex
{
public:
    complex (double r=0,double i=0);
    const complex operator+(const complex& c); // Binary
    const complex operator-(); // Prefix unary
    complex& operator+=(const complex& c);//Binary,assign
    void print() const;
private:
    double real, imag;
};
const complex complex::operator+(const complex& c){
    double r = real+c.real;
    double i = imag+c.imag;
    return complex(r,i);
}
const complex complex::operator-(){
    return complex(-real,-imag);
}
compelx& complex::operator+=(const complex& c){
    real = real+c.real;
    imag = imag+c.imag;
    return *this;
}
```

![image-20260411173332518](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260411173332518.png)

## Friend Functions

友元函数重载的好处是友元函数可以实现数字+对象的操作

```cpp
class complex
{
public:
    complex (double r=0,double i=0);
    friend const complex operator +(const complex& c1, const complex& c2); // Binary
    friend const complex operator -(const complex& c);
    // Prefix unary
    friend complex& operator+=(complex& c1, const complex& c2);//Binary,assign
    void print() const;
private:
	double real, imag;
};
complex::complex (double r, double i){ 
    real=r;
	imag=i;
}
// not member
const complex operator+(const complex& c1, const complex& c2){ 
    double r=c1.real+c2.real;
	double i=c1.imag+c2.imag;
	return complex(r,i);
}
const complex operator -(const complex& c){
	return complex(-c.real,-c.imag);
}
complex& operator+=(complex& c1, const complex& c2){
    c1.real+=c2.real;
    c1.imag+=c2.imag;
    return c1;
}
void complex::print() const{
	cout<<"(" <<real<<", "<<imag<<")"<<endl;
}
```

另一个非常经典的例子：重载<<的时候必须用friend

```cpp
class complex {
public:
    friend ostream& operator<<(ostream& os, const complex& c);
};

ostream& operator<<(ostream& os, const complex& c) {
    os << c.real << "+" << c.imag << "i";
    return os;
}
```

![image-20260411180106580](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260411180106580.png)

If the first operand is an object of an class, the operator should be declared as a member function. Otherwise, it should be declared as a friend function.

## Special Operators

以下的操作符只能被重载为成员函数

![image-20260411191554287](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260411191554287.png)

## Increment & Decrement

```cpp
Increase Increase::operator++()//prefix
{
    cout<<"++value"<<endl;
    ++value;
    return *this;
}
Increase Increase::operator++(int)//postfix
{
    cout<<"value++"<<endl;
    value++;
    return *this;
}
```

课件上这个函数没有使用引用型返回，但事实上我们应该写`Increase& Increase::operator`

prefix需要传参是占位符，无实际意义

## Overloading Assignment

重载`=`使得自定义类型可以赋值

```cpp
Date t1(2025, 4, 11);
Date t2 = t1;  // 拷贝构造：t2刚诞生
Date t3;       // 先调用默认构造
t3 = t2;       // 赋值运算符：t3已经存在，现在改它的值
```



```cpp
class A {
    int* p;
public:
    A(int i) { p = new int(i); }
    ~A() { delete p; }
    // 编译器自动生成浅拷贝赋值运算符
};

int main() {
    A a(5);
    A b(10);
    b = a;  // 浅拷贝：b.p = a.p，两个指针都指向5
    // 程序结束时，a和b都会析构，同一块内存被delete两次，直接崩溃！
    // 而且b原来指向的10的内存永远丢失了，内存泄漏
}
```

**只要类有动态分配的资源（指针成员、文件句柄、网络连接等），就必须手动写深拷贝的赋值运算符，同时也要写拷贝构造函数。**

```cpp
class A {
    int* p;
public:
    A(int i) { p = new int(i); }
    // 拷贝构造函数（必须和赋值运算符一起写）
    A(const A& rhs) {
        p = new int(*rhs.p);
    }
    // 赋值运算符重载
    A& operator=(const A& rhs) {
        if (this == &rhs) return *this; // 自赋值检查
        delete p;                       // 释放自己原来的内存
        p = new int(*rhs.p);            // 深拷贝
        return *this;                   // 返回自己
    }
    ~A() { delete p; }
};
```

## Automatic type conversion

关于构造函数转换可以参考前文中的虚数类例子

如果不想自动转换加入explicit

```cpp
class complex {
public:
    // 👇 加了 explicit，禁止隐式转换
    explicit complex(double r = 0, double i = 0) : real(r), imag(i) {}
};

int main() {
    complex c1(3.5, 5.5);
    complex c2 = c1 + 1.5;  // ❌ 编译报错！禁止隐式转换
    complex c3 = c1 + complex(1.5, 0);  // ✅ 必须显式写
}
```

### 转换运算符

构造函数只能让其他类转化成当前类，但不能把当前类转化成其他类，这时候我们就要用转换运算符
```cpp
class complex {
private:
    double real, imag;
public:
    complex(double r = 0, double i = 0) : real(r), imag(i) {}
    
    // 👇 转换运算符：把 complex 转成 int
    operator int() {
        return static_cast<int>(real);  // 返回实部的整数部分
    }
};

int main() {
    complex c(3.5, 5.5);
    int x = c;  // ✅ 自动调用 operator int()，x = 3
    cout << x << endl;  // 输出 3
}
```

# 13.Dynamic Object Creation

```cpp
#include <iostream>
using namespace std;
class Tree {
int height;
public:
Tree(int treeHeight=0) : height(treeHeight)
{cout<<"Constructor:"<<height<<endl;}
void print() { cout << height<<endl; }
};
void main( )
{
Tree* t = new Tree[3];
t[2]=Tree(40);
t[2].print( ); // 40
delete[] t;
}
Constructor:0
Constructor:0
Constructor:0
Constructor:40
40
```

## 对象创建的本质

C++ 中创建对象必然触发两个步骤：

1. 为对象分配存储空间（可选位置：静态存储区、栈、堆）
2. 调用构造函数初始化该存储空间

## 单个对象的`new`/`delete`

### 核心语法

- 创建：`new 类型(初始化器);`（无初始化器时调用默认构造函数）
- 销毁：`delete 指针名;`



## 对象数组的`new`/`delete`

### 核心语法

- 创建：`new 类型[数组大小];`
- 销毁：`delete [] 指针名;`（**必须加`[]`**，否则只会销毁第一个元素，导致内存泄漏）

### 关键特性

1. `new[]`会为数组中**每个元素自动调用默认构造函数**
2. `delete[]`会为数组中**每个元素自动调用析构函数**
3. 任何由`new`创建的对象 / 数组，必须由对应的`delete`/`delete[]`销毁
4. 同一个指针**只能被`delete`/`delete[]`执行一次**，重复释放会导致程序错误

# 14.Inheritance & Composition

本部分讨论代码复用的两种形式：

## Composition

在新类中创建已有类的对象

```cpp
//Useful.h
#ifndef USEFUL_H
#define USEFUL_H
class X{
    int i;
public:
    X(){i=0;}
    void set(int ii){i=ii;}
    int read() const{return i;}
    int permute(){return i*47;}
};
#endif

//Composition.cpp
#include"Useful.h"
class Y{
    int i;
public:
    X x;
    Y(){i=0;}
    void f(int ii){i=ii;}
    int g()const{return i;}
};

int main(){
    Y y;
    y.f(47);
    y.x.set(37);
}
```

## Inheritance

```cpp
//Inheritance.cpp
#include"Useful.h"
#include<iostream>
using namespace std;
class Y:public X{
    int i;//Different from X's i
public:
    Y(){i = 2;}
    int change{
        i = permute();//Different name call
        return i;
    }
    void set(int ii){
        i = ii;
        X::set(ii);//Same name function call
    }
}
int main( )
{
cout << "sizeof(X) = " << sizeof(X) << endl;
cout << "sizeof(Y) = " << sizeof(Y) << endl;
Y D; // X::i=1, Y::i=2
D.change(); // return Y::i=47 (X::i=47)
// X function interface comes through:
D.read(); //X::i=47
D.permute(); //X::i=47*47
// Redefined functions hide base versions:
D.set(12); // Y::i=12, X::i=12
return 0;
} ///:~
```

Y is a derived class and X is a base class

基类必须先定义，不然没法用

### The constructor initializer list

构造函数和析构函数不会被继承，赋值运算也不会被继承

子类的构造函数无法访问父类的private

所以我们引出以下解决子类构造函数访问父类private的方法

```cpp
class X {
    int a;
public:
    X(int i) : a(i) { }  // X 没有默认构造
};

class Y {
    int b;
    X x;  // 成员对象
public:
    // 必须用初始化列表给 x 传参
    Y(int i, int j) : b(i), X(j) { }
};
```

## Combining compostition & inheritance

```cpp
class X {
    int a;
public:
    X(int i=0): a(i) {cout<<"Constructor X:"<<a<<endl;}
};
class Y: public X {  // 1. 继承：Y是X的子类
    int b;           // 普通成员
    X x1, x2;        // 2. 组合：Y里包含2个X对象x1、x2（声明顺序：x1在前，x2在后）
public:
    // 初始化列表：顺序是 b(i), x2(j), x1(m), X(n)
    Y(int i, int j, int m, int n): b(i), x2(j), x1(m), X(n)
    {cout<<"Constructor Y:"<<b<<endl;}
};
int main() {
    Y y(1,2,3,4);  // 创建Y对象y，传参i=1, j=2, m=3, n=4
    return 0;
}
Constructor X: 4  → ① 基类X子对象构造
Constructor X: 3  → ② 成员对象x1构造
Constructor X: 2  → ③ 成员对象x2构造
Constructor Y: 1  → ④ Y自己的构造函数体执行
```

先构造基类->按照声明顺序构造成员对象->构建自身

## Name hiding

| 概念                    | 英文全称    | 核心定义                                                     | 作用域                      | 关键特征                                                 |
| ----------------------- | ----------- | ------------------------------------------------------------ | --------------------------- | -------------------------------------------------------- |
| **重载 (Overloading)**  | Overloading | 同一作用域内，函数名相同、参数列表不同（个数 / 类型 / 顺序） | **同一个类 / 同一个作用域** | 不影响继承，仅在本类生效，编译器根据参数自动匹配         |
| **重定义 (Redefining)** | Redefining  | 继承中，子类重写父类的**普通（非虚）成员函数**               | 父子类不同作用域            | 子类同名函数会**隐藏**父类所有同名函数，无论参数是否相同 |
| **重写 (Overriding)**   | Overriding  | 继承中，子类重写父类的**虚（virtual）成员函数**              | 父子类不同作用域            | 是多态的基础，要求函数签名完全一致，不会隐藏父类版本     |

```cpp
class Derived1 : public Base
{
public:
    // 只重定义了g()，完全没碰f()
    void g() const {}
};
class Derived2 : public Base
{
public:
    // Redefinition: 完全重写了父类的无参f()（签名完全一致）
    int f() const
    {
        cout << "Derived2::f()\n";
        return 2;
    }
};
class Derived3 : public Base
{
public:
    // Change return type: 把返回值从int改成void，函数名还是f()
    void f() const
    {
        cout << "Derived3::f()\n";
    }
};
class Derived4 : public Base
{
public:
    // Change argument list: 把参数从无参/string改成int，函数名还是f()
    int f(int) const
    {
        cout << "Derived4::f()\n";
        return 4;
    }
};
```

### Choosing composition vs inheritance

![image-20260412133031660](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260412133031660.png)

## Access Control

| 继承方式                  | 派生类内部访问基类          | 派生类对象访问基类 | 核心特点                   | 对应关系                    |
| ------------------------- | --------------------------- | ------------------ | -------------------------- | --------------------------- |
| **public（公有继承）**    | 可访问 `public`/`protected` | 仅可访问 `public`  | 基类接口完全暴露给外部     | **is-a（是一个）**，最常用  |
| **protected（保护继承）** | 可访问 `public`/`protected` | **不可访问任何**   | 基类接口只对后代派生类可见 | 几乎不用                    |
| **private（私有继承）**   | 可访问 `public`/`protected` | **不可访问任何**   | 基类接口完全对外隐藏       | **has-a（有一个）**，极少用 |

### public



### private

无论什么继承方式，子类都可以访问父类的public和protected

private修饰符只是使得父类的protected&public变成外部无法访问

### protected

protect修饰符在没有继承的时候应该与private没有任何区别

派生类内部成员可以访问，但外部不可访问

### example

```cpp 
class Location{
public:
    void InitL(int xx,int yy){
        X = xx;
        Y = yy;
    }
    void Move(int xOff,int yOff){
        X += xOff;
        Y += yOff;
    }
    int GetX(){return X;}
    int GetY(){return Y;}
private:
    int X,Y;
};
//Example1:
class Rectangle:public Location{
public:
    void InitR(int x,int y,int x,int h);
    int GetH(){return H;}
    int GetW(){return W;}
private:
    int H,W;
};
void Rectangle::InitR(int x,int y,int h,int w){
    InitL(x,y);
    H=h;
    W=w;
}
//Example2:
class V:public Rectangle{
public:
    void Function(){
        Move(3,2);
    }
};
//这个时候如果V改为private继承，那么依然可以使用Move了；但如果是Rectangle改为private继承就不可以了

//Example3
class Rectangle:private Location{
public:
    void InitR(int x,int y,int w,int h);
    int GetH(){return H;}
    int GetW(){return W;}
private:
    int H,W;
};
void Rectangle::InitR(int x,int y,int w,int h){
    InitL(x,y);//ok, it's direct inheritance
    H=h;
    W=w;
}
```



![image-20260412162313597](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260412162313597.png)

针对以上继承私有的修改方法，我们在rectangle的public部分手动包装（转发）父类的函数：

```cpp
class Rectangle: private Location {
public:
    void InitR(int x,int y,int w,int h);
    // 手动转发父类的public接口
    void Move(int xOff,int yOff) { Location::Move(xOff,yOff); }
    int GetX() { return Location::GetX(); }
    int GetY() { return Location::GetY(); }
    int GetH() { return H; }
    int GetW() { return W; }
private:
    int W,H;
};

//Example4
class Base {
public:
    void f1();
protected:
    void f3();
};

// Derived1 保护继承 Base
class Derived1 : protected Base {};

// Derived2 公有继承 Derived1
class Derived2 : public Derived1 {
public:
    void fun() {
        f1(); // ✅ ok
        f3(); // ✅ ok
    }
};

int main() {
    Derived1 d;
    d.f1(); // ❌ error
    d.f3(); // ❌ error
    return 0;
}
```

## Operator overloading & inheritance

除了上文提及的赋值运算符，其他运算符重载都会被继承到子类

## Multiple Inheritance

```cpp
#include<iostream>
using namespace std;
class B1{
    int b1;
public:
	B1(int i){
        b1 = i;
        cout<<"Constructor B1:"<<b1<<endl;
    }
    void Print(){cout<<b1<<endl;}
};
class B2
{
public:
	B2(int i) {
        b2=i; cout<<"Constructor B2:"<<b2<<endl;
    }
	void Print() {
        cout<<b2<<endl;
    }
private:
	int b2;
};
class B3
{
public:
	B3(int i) {
        b3=i; cout<<"Constructor B3:"<<b3<<endl;
    }
	int Getb3() {
        return b3;
    }
private:
	int b3;
};
class A:public B1,public B2{
    B3 bb;//member obj
    int a;
public:
    A(int i,int j,int k,int l);
    void Print();
};
A::A(int i,int j,int k,int l):a(l),bb(k),B2(j),B1(i);{
    cout<<"Constructor A:"<<a<<endl;
}
void A::Print(){
    B1::Print();
    B2::Print();
    cout<<bb.Getb3()<<endl<<a<<endl;
}
```

两个父类的构造顺序是按照继承时的顺序来的

![image-20260412170852112](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260412170852112.png)

## Incremental development

### Ambiguity

当一个类的两个父类含有同名成员函数的时候，就会ambiguity，我们应该使用`::`

```cpp
class A{
public:
	void f();   
};
class B{
public:
	void f();
    void g();
};
class C:public A,public B{
public:
    void f();
};
//这时候如果在main中直接用c.f()会发生ambiguous错误
//1
void main(){
    C c;
    c.A::f();
}
//2
class C:public A,public B{
public:
    void f();
};
void C::f(){
    A::f();
}
//这样重新定义一下也是可以的
```

也有可能存在下面这种情况：

![image-20260412163947448](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260412163947448.png)

我们有三种方法，两种跟刚才的双重继承一致，第三种是virtual base class

```cpp
class D:virtual public A,public B,virtual public C{
    
};
```

这样继承就会被优化

![image-20260412164114150](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260412164114150.png)

对于虚基类的继承：

It is called only once.（只被调用一次）

Only from the most derived class.（只能由 “最派生类” 调用）

Called before non-virtual bases.（在构造非虚基类**之前**调用）

Listed in all derived classes' initializers.（所有派生类都必须列出它）

If omitted, use default constructor.（省略则隐式调用默认构造）

![image-20260412165101504](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260412165101504.png)

```cpp
#include <iostream>
using namespace std;
class A
{
public:
    A(const char *s) { cout<<"Class A:"<<s<<endl; }
    ~A() {}
};
class B: virtual public A
{
public:
    B(const char *s1,const char *s2): A(s1)
    { cout<<"Class B:"<<s2<<endl; }
};
class C: virtual public A
{
public:
    C(const char *s1,const char *s2): A(s1)
    { cout<<"Class C:"<<s2<<endl; }
};
class D: public B,public C
{
public:
    D(const char *s1,const char *s2,
    const char *s3,const char *s4)
    : C(s2,s3), B(s4,s2), A(s1)
    { cout<<"Class D:"<<s4<<endl; }
};
void main()
{
    D *ptr=new D("str1", "str2", "str3", "str4");
    delete ptr;
}

```

## Upcasting

让基类的指针可以指向派生类

```cpp
// 基类：乐器
class Instrument {
public:
    void play() const {} // 乐器的通用接口：演奏
};

// 子类：管乐器，公有继承Instrument（满足is-a：管乐器是一种乐器）
class Wind : public Instrument {};
// 给乐器调音的函数，接收Instrument&类型的参数
void tune(Instrument& i) { 
    i.play(); // 调用乐器的play方法
}

```

# 15.Polymorphism & Virtual Functions

接着上面的upcasting 

```cpp
class Instrument {
public:
    void play( ) const  {cout<<"Instrument::play" << endl;  }
};
// Wind objects are Instruments
// because they have the same interface:
class Wind : public Instrument {
public:
    // Redefine interface function:
    void play( ) const  { cout << "Wind::play" << endl; }
};
void tune( Instrument& i ) {  i.play( );  }
void main()
{
    Wind flute;
    tune(flute); // Upcasting
}
```

这种情况的`Wind::play`没有成功调用，只调用了`Instrumental::play`

我们要用虚函数解决这个问题

## Virtual functions

The redefinition of a virtual function in a derived class is usually called overriding

Polymorphism:  Different functions with the same name. 

Virtual Functions(overriding) : dynamically determined

Function overloading: statically determined

| 特性 | Function Overloading | Virtual Function |
| ---- | -------------------- | ---------------- |
| 时间 | 编译期               | 运行时           |
| 绑定 | static binding       | dynamic binding  |
| 条件 | 参数不同             | 继承 + virtual   |
| 本质 | 名字复用             | 行为多态         |

```cpp
//: C15:Instrument2.cpp
// Inheritance & upcasting
#include <iostream>
using namespace std;

class Instrument 
{
public:
    virtual void play( ) const 
    {  cout << "Instrument::play" << endl;  }
};
// Wind objects are Instruments
// because they have the same interface:

class Wind : public Instrument 
{
public:
    // Redefine interface function:
    virtual void play( ) const //no “virtual” is ok.
    {  cout << "Wind::play" << endl;  }
};

void tune( Instrument& i )    {   i.play( );  }
void main()
{
    Wind flute;
    tune(flute); // Upcasting
}
```

another

```cpp
// Extensibility in OOP
class Instrument {
public:
  virtual void play() const { cout << "Instrument::play" << endl;}
  virtual char* what() const {return "Instrument"; }
  // Assume this will modify the object:
  virtual void adjust(int) {}
};

class Wind : public Instrument {
public:
  void play() const { cout << "Wind::play" << endl; }
  char* what() const { return "Wind"; }
  void adjust(int) {}
};

class Stringed : public Instrument {
public:
  void play() const {cout << "Stringed::play" << endl;}
  char* what() const { return "Stringed"; }
  void adjust(int) {}
};
class Brass : public Wind {
public:
  void play() const { cout << "Brass::play" << endl; }
  char* what() const { return "Brass"; }
};

// Identical function from before:
void tune(Instrument& i) { i.play( );}

// New function:
void f(Instrument& i) { i.adjust( 1); }

void main() {
  Wind flute;
  Stringed violin;
  Brass horn;
  tune(flute); //Wind::play
  tune(violin); // Stringed::play
  tune(horn); // Brass::play
  f(horn); // Wind::adjust
}
```

1. A Virtual function is a nonstatic member function.
2. If a virtual function is defined outside the class body, keyword virtual is only needed when declared.

```cpp
class Instrument 
{
public:
    virtual void play( ) const; 
};
void Instrument :: play( ) const
{
    cout << "Instrument::play" << endl;
}
```

3.When using scope resolution operator ::, virtual mechanism will not be used.

![image-20260412173459218](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260412173459218.png)

虚函数与他的同名函数必须是同一类的

**只有通过基类指针/引用才会触发运行时多态**

![image-20260412173853411](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260412173853411.png)

```cpp
//: C15:Early.cpp
// Early binding & virtual functions
#include <iostream>
#include <string>
using namespace std;
class Pet {
public:
    virtual string speak() const { return "Pet:: speak"; }
};

class Dog : public Pet {
public:
   virtual string speak() const { return "Dog::speak"; }
};
void main( ) 
{
    Dog ralph;
    Pet* p1 = &ralph; // type ambiguity 
    Pet& p2 = ralph;   // type ambiguity 
    Pet p3 = ralph;	     // no ambiguity 
    // Late binding for both:
    cout<<p1->speak() <<endl;
    cout<<p2.speak() << endl;
    // Early binding:
    cout<<p3.speak() << endl;
} ///:~

```

![image-20260412174115556](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260412174115556.png)

## Abstract base classed and pure virtual functions

抽象类最少有一个纯虚函数

抽象类不可实例化

对于抽象类的所有指针都会upcasting

```cpp
#include <iostream>
using namespace std;
class Point   //  abstract class
{
public:
    Point(int i=0,int j=0) {x0=i;y0=j;}
    virtual void Set()=0;
    virtual void Draw()=0;
protected:
    int x0,y0;
};
class Line: public Point
{
public:
    Line(int i=0,int j=0,int m=0,int n=0): Point(i,j)
        	  {x1=m;y1=n;}
    virtual void Set() 
        	  {cout<<"Line::Set() called. "<<endl;}
    virtual void Draw() 
        	  {cout<<"Line::Draw() called. "<<endl;}
protected:
    int x1,y1;
};
//  abstract class
class Ellipse: public Point
{
public:
    Ellipse(int i=0,int j=0,int p=0,int q=0):Point(i,j)
                {x2=p;y2=q;}
protected:
    int x2,y2;
};
void main()
{
    Line line(0,1);
    // Ellipse ellipse(0,1,2,3); // error
    Point& p=line;
    p.Set();
    p.Draw();
}
```

## Inheritance and the VTABLE

子类 VTABLE = 基类 VTABLE 的“复制 + 修改”

如果子类override，则替换成子类的函数地址

如果子类没override，则沿用父类的地址

```cpp
//: C15:AddingVirtuals.cpp, VTABLE
class Pet {
  string pname;
public:
  Pet(const string& petName) : pname(petName) {}
  virtual string name() const { return pname; }
  virtual string speak() const { return ""; }
};
class Dog : public Pet {
  string name;
public:
  Dog(const string& petName) : Pet(petName) {}
  virtual string sit() const { return Pet::name() + " sits"; } // New virtual function 
  string speak() const{ return Pet::name() + " says 'Bark!'";  } // Override
};
int main() {
  Pet* p[] = {new Pet("generic"), new Dog("bob")};
  cout << "p[0]->speak() = " << p[0]->speak() << endl;
  cout << "p[1]->speak() = " << p[1]->speak() << endl;
 //! cout << "p[1]->sit() = " << p[1]->sit() << endl; // Illegal
  delete p[0]; delete p[1];
} ///:~

```

![image-20260412203515316](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260412203515316.png)

```cpp
Dog(const string& petName) : Pet(petName) {}
                               ↑
                           这里就是初始化列表
```

### Object Slicing

Upcasting by value is unsafe.

```cpp
//: C15:ObjectSlicing.cpp
#include <iostream>
#include <string>
using namespace std;

class Pet 
{
  string pname;
public:
  Pet(const string& name) : pname(name) {}
  virtual string name( ) const { return pname; }
  virtual string description( ) const 
  		{ return "This is " + pname;  }
};
class Dog : public Pet 
{
  string favoriteActivity;
public:
  Dog(const string& name, const string& activity)
    : Pet(name), favoriteActivity(activity) {}
   virtual string description( ) const 
  { return Pet::name( )+" likes to "+favoriteActivity; }
};

void describe(Pet a)  // Slices the object
{  
   cout << a.description() << endl; 
}
void main( ) 
{
  Pet p("Zhang");
  Dog d("Li", "sleep"); 
  describe(p);
  describe(d); // Pet::pname, name(), description( ) 
} ///:~
This is Zhang 
This is Li   ?
```

`describe(d)`的时候我们的d依然是pet的方法，而d是dog对象，那么我们就无法表示dog自己的部分`sleep`

## Overloading & Overriding

```cpp
//: C15:NameHiding2.cpp
class Base {
public:
  virtual int f() const {cout<<"Base::f()\n";return 1;}
  virtual void f(string) const {}
  virtual void g() const {}
};
class Derived1 : public Base {
public:   void g() const {}
};
class Derived2 : public Base {
public: // Overriding 
    int f() const {cout<<"Derived2::f()\n"; return 2;}
};
class Derived3 : public Base {
public: // Cannot change return type
//! void f() const{ cout << "Derived3::f()\n";}
};
class Derived4 : public Base {
public:
 int f(int) const { cout << "Derived4::f()\n"; return 4;} // Change argument list
};
int main() {
  string s("hello");
  Derived1 d1;
  int x = d1.f();
  d1.f(s);
  Derived2 d2;
  x = d2.f();
//! d2.f(s); // string version hidden
  Derived4 d4;
  x = d4.f(1);
//! x = d4.f(); // f() version hidden
//! d4.f(s); // string version hidden
  Base& br = d4; // Upcast
//! br.f(1); // Derived version unavailable
  br.f(); // Base version available
  br.f(s); // Base version abailable
} ///:~

```

## Virtual functions & Constructors

- When an object containing virtual functions is created, its VPTR must be initialized to point to the proper VTABLE. 

- The constructor initializes the VPTR.The constructor cannot be virtual.

对于一个构造过程，我们先构造他的vptr，让vptr指向父类的vtable并修改为继承类的vtable

构造函数不能是虚函数，因为构造函数负责构造vptr

![image-20260412205641669](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260412205641669.png)

## Destructors and virtual destructors

- The destructor can and often must be virtual.
- If the destructor in the base is declared virtual, destructors in derived classes are all virtual even without the keyword virtual.
- To ensure destructors can be called exactly.
![image-20260412211027289](C:\Users\zhang\AppData\Roaming\Typora\typora-user-images\image-20260412211027289.png)

如果基类析构函数是 virtual，那么所有派生类析构函数自动也是 virtual（即使不写 virtual）

## Operator overloading

运算符重载可以向其他任何函数一样virtual

## Downcasting

unsafe!

# 16.Introduction to Templates

### syntax

```cpp
template<typename T>
T max(T x,T y){
    return (x>y)?x:y;
}
```

eg

```cpp
#include <iostream>
using namespace std;
template < typename T> 
T max (T x,T y)  //function template
{   return (x>y) ? x : y ;  }
void main()
{
    int x1(1),y1(2);
    double x2(3.4),y2(5.6);
    char x3='a',y3='b';
    cout<<max(x1,y1)<<endl; //T is instantiated by int
    cout<<max(x2,y2)<<endl; //T is instantiated by double
    cout<<max(x3,y3)<<endl; //T is instantiated by char
}

```

```cpp
#include <iostream>
using namespace std;

template <typename T1, typename T2> 
T1 max (T1 x,T2 y)
{   return (x>T1(y)) ? x : T1(y) ;  }

void main()
{
    cout<<max (3,  5.6)<<endl; 
    cout<<max (5.6,  3)<<endl; 
    cout<<max (‘a’,  3)<<endl; 
}

```

## Class Templates

```cpp
	template < Type argument list>
	class name
	{
	    //the body of class
	}

```

```cpp
#include <iostream>
using namespace std;
template <typename T1, typename T2>  
class Test
  {
      public:
          Test(T1 x,T2 y):a(x),b(y) { };
          void Print();
      private:
          T1 a;
          T2 b;
  }; 
  // member function 
template <typename T1, typename T2> 
void Test<T1,T2>:: Print()
  {   cout<<a<<", "<<b<<endl;   }

```

