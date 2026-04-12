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

澹版槑鎸囧嚭鍚嶅瓧锛屽畾涔夊垎閰嶇┖闂?

澹版槑鍙互鎵ц澶氭锛屼絾瀹氫箟鍙兘鎵ц涓€娆★紱

浣跨敤`extern`琛ㄧず璇ヨ鍙ヤ粎浠呮槸澹版槑锛岃€屽畾涔夊湪闅忓悗/澶栭儴鏂囦欢锛?

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

鎴戜滑姝ゅ鐨刞int i;//E`鏄畾涔?澹版槑锛屾墍浠ユ垜浠鏋滈渶瑕佸湪鏌愬鎸囧畾鏄０鏄庯紝蹇呴』鐢╜extern`浠ラ槻姝㈠湪澶氭枃浠朵腑閲嶅瀹氫箟銆?

## Linking

1. 棣栧厛鏄痯reprocessor锛宲reprocessor鎶奿nclude鐨勫ご鏂囦欢澶嶅埗杩涙潵锛屾妸瀹忓畾涔夋浛鎹紱
2. 鍏舵鏄痗ompiler锛屽叾涓€鏄痯arsing锛氭妸浠ｇ爜鍙樻垚璇硶鏍戯紱鍏朵簩鏄痵ynthesizing锛氭妸璇硶涔﹀彉鎴愭眹缂?鏈哄櫒浠ｇ爜锛屽苟寰楀埌涓€涓猔.o/.obj`鏂囦欢锛?
3. 绗笁姝ユ槸Linker锛屼粬鎶婃墍鏈塦.o`杩炴帴璧锋潵锛屽苟瀵绘壘鍚勫紩鐢ㄤ綅缃紱

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

Vector 鏄竴涓猼emplate

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

cpp鍏佽绌烘寚閽坄void*`鐨勫瓨鍦紝浣嗕笉鍏佽缁欐湁绫诲瀷鎸囬拡璧嬬┖鎸囬拡鍊硷細

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

- Static storage area:鍦ㄧ▼搴忚繍琛屽墠灏辫鍒嗛厤
- Stack:CPU鐩存帴鐢ㄦ潵瀛樺偍杩愯鏁版嵁(microprocessor)
- Heap:鍔ㄦ€佸唴瀛樺垎閰?

### Global

global榛樿鏄彲external linkage鐨勶紝涔熷氨鏄榛樿`int globe;` == `extern int globe;`

杩欏彲浠ヤ繚璇佸涓枃浠朵腑鐨刧lobe閮芥寚鍚戝悓涓€涓湴鍧€

```cpp
```

### Local

鍦╯tack

no linkage

### Static

static鍙垵濮嬪寲涓€娆★紝鑰屼笖鐢熷瓨鍛ㄦ湡鏄暣涓▼搴?

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

static 鍙橀噺涓嶅澶栨毚闇诧紝鏃犳硶璺ㄦ枃浠惰闂?

### Extern



### Constant&Volatile

Volatile:寮哄埗缂栬瘧鍣ㄦ瘡娆′粠鍐呭瓨涓噸鏂拌鍙栧彉閲忥紙杩欏彲浠ラ闃插彉閲忚澶栭儴鍔涢噺淇敼锛?

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

**閫楀彿杩愮畻绗︿細渚濇鎵ц澶氫釜琛ㄨ揪寮忥紝浣嗘渶缁堢粨鏋滄槸鈥滄渶鍚庝竴涓〃杈惧紡鐨勫€尖€?*

绗簩涓緥瀛愶紝鍥犱负閫楀彿鐨勪紭鍏堢骇寰堜綆锛屾墍浠ヤ簨瀹炴槸锛歚(a=b),c,d,e;`

## Casting operator

寮哄埗绫诲瀷杞崲锛?

```cpp
int a = 100;
float b = float(a);
float c = (float)a;
int d = int(5.5);
```

## Function address

鍐欐垚`void (*fp)();`

璧嬪€肩殑鏃跺€欏嚱鏁板悕浼氳嚜鍔ㄩ€€鍖栨垚鍑芥暟鎸囬拡锛?

鍑芥暟鎸囬拡鐨勭被鍨嬩笌鍑芥暟杩斿洖绫诲瀷涓€鑷?

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

浣跨敤`new`  `delete`鍒嗛厤鍜岄攢姣佺┖闂?



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

class鐨勯粯璁ょ姸鎬佹槸private

鑻ヨ璁块棶绫荤殑鏂规硶锛?

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

闈為潤鎬佹垚鍛樺嚱鏁扮殑棣栦釜鍙傛暟閮芥槸闅愬惈鐨則his鎸囬拡锛屽湪杩欎釜渚嬪瓙閲屾槸`Stash*`绫诲瀷

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

鏅€氱殑鎴愬憳鍑芥暟鍖呭惈涓夌偣锛氳闂畃rivate锛涘睘浜庣被浣滅敤鍩燂紱鏈塼his鎸囬拡

鑰屽弸鍏冨嚱鏁板彧鏈夌涓€鐐广€?

鈥?Asymmetric

鈥?non-transitive

鈥?cannot be inherited

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

### 鍙嬪厓浣滀负global function锛?

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

浠ヤ笂渚嬪瓙鏄痝lobal function as a friend锛涙垜浠篃鏈塯lobal function as friends锛屾剰鍛崇潃涓€涓嚱鏁板彲浠ユ槸澶氫釜绫荤殑鍙嬪厓銆?

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
    cout << "Boy鈥檚 name is " << name << endl
        << "Girl鈥檚 name is " << x.name << endl;
}
```

### a class as a friend

鍦ㄤ笅闈㈢殑渚嬪瓙涓紝灏辨槸Y涓殑鎵€鏈夋柟娉曢兘鍙互璁块棶X鐨刾rivate鎴愬憳

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

鍏充簬闈欐€佹垚鍛樼殑璁块棶锛?

```cpp
class Test{
public:
    static int count;
};
//璋冪敤锛?
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

- constructor鐨勭涓€涓殣鍚弬鏁版槸this鎸囬拡锛?

- constructor鍙互鏈夊弬鏁帮紱

- constructor鍙互琚玱verloaded锛?

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

鎴戜滑涔熷彲浠ユ湁澶氫釜constructor

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

涓嬮潰鐨勬槸default constructor

```cpp
A(){}
```

## Cleanup with the destructor

鈥?A destructor has not formal arguments andcannot be overloaded.

鈥?A destructor has no return.

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

![image-20260410203133108](img/oop_assets/image-20260410203133108.png)

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

鍙互閫氳繃涓€涓嬫柟寮忓垵濮嬪寲object arrays

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

![image-20260410204118271](img/oop_assets/image-20260410204118271.png)

# 7.Function Overloading & Default Arguments

褰撴湁浜涘嚱鏁板湪涓嶅悓鐨勭被涓婃墽琛岀浉鍚岀殑浠诲姟鏃讹紝鎴戜滑鍊惧悜浜庣粰杩欎簺鍑芥暟鐩稿悓鐨勫悕瀛?

![image-20260410204437378](img/oop_assets/image-20260410204437378.png)

杩欎釜杩囩▼灏辨槸overloading

缂栬瘧鍣ㄤ細鎸夌収鍙傛暟鐨勭被鍨?鏁伴噺閫夋嫨鏈€鍚堥€傜殑鍑芥暟锛岃繖涓繃绋嬪苟涓嶄細鑰冭檻鍒板弬鏁扮殑鍚嶇О&杩斿洖绫诲瀷

![image-20260410204850488](img/oop_assets/image-20260410204850488.png)

骞朵笖缂栬瘧鍣ㄨ繕浼氳€冭檻鍒癰uilt-in type cast锛岀湅鐪嬭浆鎹㈠悗鐨勬暟鎹槸鍚︽弧瓒冲嚱鏁拌姹?

## Default Arguments

```cpp
void print(int day=1){cout<<day<<'#';}
int main(){
    cout<<"Day";
    print();
    return 0;
}
```

榛樿鍙傛暟蹇呴』鏀惧湪鏈€鍚?

鎴戜滑鍙兘鍐欏涓嬭〃杈撅細

```cpp
void fun(int a=1,int b=3,int c=5);
......
void fun(int a, int b, int c){
    ......
}
//鑰屼笉鑳藉湪绗簩娆″畾涔夊嚱鏁扮殑鏃跺€欏啀鍐欎竴閬嶉粯璁ゅ弬鏁帮紝涓嶇劧浼歳edefinition error
```

- **榛樿鍙傛暟鏄潤鎬佺粦瀹氱殑锛?* 缂栬瘧鍣ㄥ湪缂栬瘧鏌愪釜 `.cpp` 鏂囦欢鏃讹紝鍙湅璇ユ枃浠朵腑鍙鐨勫嚱鏁板０鏄庢潵鍐冲畾榛樿鍙傛暟濉粈涔堛€?

![image-20260410212346862](img/oop_assets/image-20260410212346862.png)

![image-20260410212443380](img/oop_assets/image-20260410212443380.png)

# 8.Constants

## Pointers

### CONSTANT&POINTER

```CPP
const 鍦ㄦ寚閽堝乏杈硅〃绀烘寚閽堟寚鍚戠殑鍐呭鏄父閲?
const 鍦?鍙宠竟琛ㄧず鎸囬拡鏈韩鏄父閲?
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

浠ヤ笂浠ｇ爜鎬昏涓変釜閿欒锛?

- 绗竴銆佷簩涓槸鍥犱负褰撴垜浠畾涔夋寚鍚戝父閲忕殑鎸囬拡锛岄偅涔堟垜浠笉鑳介€氳繃瑙ｅ紩鐢ㄦ柟寮忎慨鏀规寚鍚戠殑鍊硷紱
- 绗笁涓槸鍥犱负鎴戜滑涓嶈兘鐢ㄤ笉鏄寚鍚戝父閲忕殑鎸囬拡鎸囧悜甯搁噺锛?=浣嗘槸甯搁噺鎸囬拡鍙互鎸囧悜闈炲父閲?=锛岀悊瑙ｄ负鎸囧悜甯搁噺鐨勬寚閽堝彧鏄姞鍏ヤ簡鍙闄愬埗銆?

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

- 绗竴涓敊璇槸鐢变簬褰撴垜浠畾涔変簡涓€涓父閲忔寚閽堬紝閭ｄ箞鎴戜滑灏卞啀涔熸棤娉曚慨鏀逛粬鐨勬寚鍚?
- 绗簩涓敊璇槸鐢变簬鎴戜滑铏界劧瀹氫箟`pv2`涓哄父閲忔寚閽堬紝浣嗘槸鍏舵寚鍚戠殑閲忎粛鐒跺彲浠ラ€氳繃璇ュ父閲忔寚閽堜慨鏀癸紝閭ｄ箞`max`鐨勫父閲忔€у氨鏃犳硶寰楀埌缁存姢銆?

### Constant Pointer to a const

```cpp
int v1 = 10, v2 = 20;
const int max = 100;
const int* const pv1 = &v1;
const int* const pv2 = &max;
pv1 = &v2;//error
*pv1 = v2;//error
```

- 绗竴涓敊璇樉鑰屾槗瑙?
- 绗簩涓敊璇湪浜庝笉鑳介€氳繃鎸囧悜甯搁噺鐨勬寚閽堜慨鏀瑰彉閲?

## Temporaries

鎴戜滑鍦ㄤ笂鏂囦腑缁橠ate绫昏祴鍊肩殑鏃跺€欐湁涓€鍙ワ細

`Date date[3] = {Date(2003,4,5),Date(2003,4,5)};`

杩欏彞涓殑`Date(2003,4,5)`灏变細鏋勯€犱复鏃跺璞★紝杩欎釜涓存椂瀵硅薄浼氳缂栬瘧鍣ㄨ涓哄父閲?

## Classes

### Static data members

褰撲綘鍦ㄧ被鍐呭畾涔変簡涓€涓潤鎬佸彉閲忔垚鍛橈紝閭ｄ箞杩欎釜绫荤殑鎵€鏈夊疄浣撻兘鍏辩敤杩欎竴涓彉閲忥紝涓嶄細鍒涢€犳柊鐨?

![image-20260410224655150](img/oop_assets/image-20260410224655150.png)

绫婚潤鎬佸彉閲忕殑姝ｇ‘瀹氫箟鏂瑰紡锛?

鍙兘鍦ㄧ被澶栧垵濮嬪寲

绫诲涓嶉渶瑕乻tatic

绫诲煙鍚?

```cpp
class Myclass{
    static int obj;
};
int Myclass::obj = 8;
```

const鏁版嵁鎴愬憳蹇呴』浣跨敤鍒濆鍖栧垪琛ㄥ垵濮嬪寲

鑰宻tatic const 搴旇鍗曠嫭澶勭悊

鑰屼笖鍒濆鍖栭『搴忕湅鐨勬槸鍦ㄧ被鍐呭畾涔夐『搴忥紝鑰岄潪鍒楄〃椤哄簭

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
A::A(int i):r(i),a(r+1);//姝ｅ鍒氭墠鎵€璇达紝浜庢槸杩欎釜璧嬪€间笉鑳芥垚鍔?
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

绫荤殑瀹炰綋鐨勭敓瀛樻湡鍐呮病鏈夋暟鎹垚鍛樿鏀瑰彉

```cpp
int main( ) {
    Date A(2003, 10, 1); // non-const object
    const Date B(2000, 9, 8); //const object
    return 0;
}
```

**甯搁噺瀵硅薄锛圕onst Objects锛夊彧鑳借皟鐢ㄢ€滄壙璇轰笉淇敼鏁版嵁鈥濈殑鏂规硶锛圕onst Member Functions锛夈€?*

### Mutable

mutable鏄父閲忓璞＄殑璞佸厤绗?

```cpp
class Date {
public:
    // ... 鏋勯€犲嚱鏁?...
    int year() const { return ++y; } // 娉ㄦ剰杩欓噷锛歝onst 鍑芥暟
private:
    mutable int y; // 娉ㄦ剰杩欓噷锛歮utable 淇グ
    int m, d;
};
```

# 9.Inline Functions

瀹忓湪鏇挎崲鐨勬椂鍊欎細鏈夊悇绉嶅悇鏍风殑闂

鎴戜滑寮曞叆inline function锛岄€氳繃inline function鍙互浣垮緱缁忓父琚皟鐢ㄧ殑鍑芥暟鍑忓皯杩愯鏃堕棿

```cpp
inline int add(int x,int y,int z){
    return x+y+z;
}
int main(){
    int a(1),b(2),c(3)sum(0);
    sum=add(a,b,c);
}
```

锛?锛夊繀椤诲湪璋冪敤鍓嶅畾涔?

锛?锛変笉瑕佹湁寰幆 / switch

锛?锛変笉瑕佹湁寮傚父澶勭悊

锛?锛変笉瑕侀€掑綊

## Class & Inline

绫诲唴瀹炵幇鐨勫嚱鏁拌嚜鍔ㄥ氨鏄痠nline

濡傛灉鍑芥暟鍦ㄧ被澶栧疄鐜帮紝閭ｄ箞闇€瑕佸姞涓奿nline鏉ヨ〃鏄庝粬鏄痠nline

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

==inline搴旇鍜岀被鐨勫畾涔夊湪鍚屼竴涓枃浠朵腑==

inline 鍑芥暟蹇呴』鍦ㄢ€滄瘡涓娇鐢ㄥ畠鐨勭炕璇戝崟鍏冿紙.cpp锛変腑閮借兘鐪嬪埌瀹氫箟

# 10.Name Control

static鍐呯疆鍨嬪彉閲忚鍒濆鍖栨垚0锛屼絾鏄痵tatic user defined types浼氳constructor瀹氫箟

![image-20260411110220662](img/oop_assets/image-20260411110220662.png)

# Controling linkage

Visibility:external linkage, internal linkage 

- External linkage: the names in file scope are visible for all translation units (.cpp files) in a program. **Global variables and ordinary functions have external linkage.** 
- Internal linkage (file static): the names are only visible for its translation unit. **Static, const and inline names default to internal linkage.**

## Namespace

| #    | namespace                                   | class/struct                      |
| ---- | ------------------------------------------- | --------------------------------- |
| 1    | **鍙兘鍏ㄥ眬鍑虹幇**锛屼笉鑳藉啓鍦ㄥ嚱鏁颁綋閲?         | 鍙互鍦ㄥ嚱鏁颁綋閲屽畾涔夊眬閮ㄧ被          |
| 2    | 缁撴潫 **涓嶇敤鍒嗗彿**                           | 缁撴潫蹇呴』鍐欏垎鍙?                   |
| 3    | **鍚屽悕 namespace 鍙媶鍒?* 鍒板涓枃浠?澶存枃浠?| 鍚屽悕 class 鍙兘瀹氫箟涓€娆?          |
| 4    | 鍙互 **璧峰埆鍚?*`namespace ALib = MyLib;`    | 璧峰埆鍚嶈鐢?`using A = SomeClass;` |
| 5    | **涓嶈兘瀹炰緥鍖?*                              | class 鍙互鏋勯€犲璞?               |

```cpp
// 鏂囦欢 a.h
namespace MyLib {
    void f();            // 鍙啓澹版槑
}

// 鏂囦欢 a.cpp
#include "a.h"
namespace MyLib {        // 鍚屽悕銆佸悓绾у埆锛岃嚜鍔ㄥ悎骞?
    void f() { ... }     // 鍐欏畾涔?
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

鎴戜滑杩樺彲浠ュ彧浣跨敤鍛藉悕绌洪棿涓殑閮ㄥ垎鍑芥暟瀵煎叆

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

杩斿洖寮曠敤锛?

```cpp
int& g(int &x){
    x++;
    return x;
}
```

杩斿洖寮曠敤鐨勫ソ澶勬槸鎴戜滑鍙互閾惧紡鎿嶄綔`g(g(a))`锛屽苟锛氬湪杩欎釜`g(a)`涓紝a琚嚜鍔ㄧ粦瀹氫簡浠栫殑寮曠敤x

## Const Reference

鏃㈠彲浠ョ粦瀹氭櫘閫氬彉閲忥紙`g(a)` 姝ｅ父杩愯锛夛紝涔熷彲浠ョ粦瀹氫复鏃跺€?/ 瀛楅潰閲忥紙`g(1)` 姝ｅ父杩愯锛夈€?

鍥犱负缂栬瘧鍣ㄤ細涓轰复鏃跺€煎垱寤轰竴涓?鈥滀复鏃跺璞♀€濓紝璁╁父寮曠敤缁戝畾鍒拌繖涓复鏃跺璞′笂锛堟櫘閫氬紩鐢ㄤ笉鍏佽杩欎箞鍋氾紝鍙湁甯稿紩鐢ㄥ彲浠ワ級銆?

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

鎴戜滑鍦ㄨ繖涓繃绋嬩腑鐩存帴淇敼浜嗘寚閽堟寚鍚戠殑鍐呭瓨

## The copy-constructor

鍐欐硶锛歚X::X(const X& .)`

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
        cout<<鈥淐opy constructor called."<<endl;
    }
    ~Date( ) { cout<<"Destructor called."<<endl; }
};
void main() {
    Date d1(2003,9,20);
    Date d2=d1;
}
```

褰撲护`d2=d1`鏋勯€犵殑鏃跺€欙紝璋冪敤鎷疯礉鏋勯€?


The Copy Constructor is called when

- A creating class object is initialized by another created object of the same class.
- passing arguments by values.
- a function returning an object value.

The Copy Constructor **is not called whenpassing arguments by references**. Because no new object is created.

![image-20260411170745301](img/oop_assets/image-20260411170745301.png)銆?

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

璋冪敤鏂瑰紡锛?

```cpp
瀵硅薄.*鎸囬拡鍚?
瀵硅薄鎸囬拡->*鎸囬拡鍚?
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

    // 瀹氫箟涓€涓寚鍚慦idget绫讳腑鎴愬憳鍑芥暟鐨勬寚閽坧mem
    // 鍑芥暟绛惧悕锛氳繑鍥瀡oid锛屽弬鏁癷nt锛宑onst淇グ
    // 璁╁畠鎸囧悜Widget::h鍑芥暟
    void (Widget::*pmem)(int) const = &Widget::h;

    // 閫氳繃瀵硅薄w璋冪敤
    (w.*pmem)(1);  // 杈撳嚭锛歐idget::h()

    // 閫氳繃瀵硅薄鎸囬拡wp璋冪敤
    (wp->*pmem)(2); // 杈撳嚭锛歐idget::h()
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

杩欎釜operator鐨勮皟鐢ㄤ簨瀹炰笂鏄痐Integer c = a+b;//a.operator+(b)`

浣跨敤寮曠敤绫诲瀷鍙互閬垮厤杩囩▼涓骇鐢熺殑鎷疯礉

![image-20260411173058700](img/oop_assets/image-20260411173058700.png)

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

![image-20260411173332518](img/oop_assets/image-20260411173332518.png)

## Friend Functions

鍙嬪厓鍑芥暟閲嶈浇鐨勫ソ澶勬槸鍙嬪厓鍑芥暟鍙互瀹炵幇鏁板瓧+瀵硅薄鐨勬搷浣?

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

鍙︿竴涓潪甯哥粡鍏哥殑渚嬪瓙锛氶噸杞?<鐨勬椂鍊欏繀椤荤敤friend

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

![image-20260411180106580](img/oop_assets/image-20260411180106580.png)

If the first operand is an object of an class, the operator should be declared as a member function. Otherwise, it should be declared as a friend function.

## Special Operators

浠ヤ笅鐨勬搷浣滅鍙兘琚噸杞戒负鎴愬憳鍑芥暟

![image-20260411191554287](img/oop_assets/image-20260411191554287.png)

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

璇句欢涓婅繖涓嚱鏁版病鏈変娇鐢ㄥ紩鐢ㄥ瀷杩斿洖锛屼絾浜嬪疄涓婃垜浠簲璇ュ啓`Increase& Increase::operator`

prefix闇€瑕佷紶鍙傛槸鍗犱綅绗︼紝鏃犲疄闄呮剰涔?

## Overloading Assignment

閲嶈浇`=`浣垮緱鑷畾涔夌被鍨嬪彲浠ヨ祴鍊?

```cpp
Date t1(2025, 4, 11);
Date t2 = t1;  // 鎷疯礉鏋勯€狅細t2鍒氳癁鐢?
Date t3;       // 鍏堣皟鐢ㄩ粯璁ゆ瀯閫?
t3 = t2;       // 璧嬪€艰繍绠楃锛歵3宸茬粡瀛樺湪锛岀幇鍦ㄦ敼瀹冪殑鍊?
```



```cpp
class A {
    int* p;
public:
    A(int i) { p = new int(i); }
    ~A() { delete p; }
    // 缂栬瘧鍣ㄨ嚜鍔ㄧ敓鎴愭祬鎷疯礉璧嬪€艰繍绠楃
};

int main() {
    A a(5);
    A b(10);
    b = a;  // 娴呮嫹璐濓細b.p = a.p锛屼袱涓寚閽堥兘鎸囧悜5
    // 绋嬪簭缁撴潫鏃讹紝a鍜宐閮戒細鏋愭瀯锛屽悓涓€鍧楀唴瀛樿delete涓ゆ锛岀洿鎺ュ穿婧冿紒
    // 鑰屼笖b鍘熸潵鎸囧悜鐨?0鐨勫唴瀛樻案杩滀涪澶变簡锛屽唴瀛樻硠婕?
}
```

**鍙绫绘湁鍔ㄦ€佸垎閰嶇殑璧勬簮锛堟寚閽堟垚鍛樸€佹枃浠跺彞鏌勩€佺綉缁滆繛鎺ョ瓑锛夛紝灏卞繀椤绘墜鍔ㄥ啓娣辨嫹璐濈殑璧嬪€艰繍绠楃锛屽悓鏃朵篃瑕佸啓鎷疯礉鏋勯€犲嚱鏁般€?*

```cpp
class A {
    int* p;
public:
    A(int i) { p = new int(i); }
    // 鎷疯礉鏋勯€犲嚱鏁帮紙蹇呴』鍜岃祴鍊艰繍绠楃涓€璧峰啓锛?
    A(const A& rhs) {
        p = new int(*rhs.p);
    }
    // 璧嬪€艰繍绠楃閲嶈浇
    A& operator=(const A& rhs) {
        if (this == &rhs) return *this; // 鑷祴鍊兼鏌?
        delete p;                       // 閲婃斁鑷繁鍘熸潵鐨勫唴瀛?
        p = new int(*rhs.p);            // 娣辨嫹璐?
        return *this;                   // 杩斿洖鑷繁
    }
    ~A() { delete p; }
};
```

## Automatic type conversion

鍏充簬鏋勯€犲嚱鏁拌浆鎹㈠彲浠ュ弬鑰冨墠鏂囦腑鐨勮櫄鏁扮被渚嬪瓙

濡傛灉涓嶆兂鑷姩杞崲鍔犲叆explicit

```cpp
class complex {
public:
    // 馃憞 鍔犱簡 explicit锛岀姝㈤殣寮忚浆鎹?
    explicit complex(double r = 0, double i = 0) : real(r), imag(i) {}
};

int main() {
    complex c1(3.5, 5.5);
    complex c2 = c1 + 1.5;  // 鉂?缂栬瘧鎶ラ敊锛佺姝㈤殣寮忚浆鎹?
    complex c3 = c1 + complex(1.5, 0);  // 鉁?蹇呴』鏄惧紡鍐?
}
```

### 杞崲杩愮畻绗?

鏋勯€犲嚱鏁板彧鑳借鍏朵粬绫昏浆鍖栨垚褰撳墠绫伙紝浣嗕笉鑳芥妸褰撳墠绫昏浆鍖栨垚鍏朵粬绫伙紝杩欐椂鍊欐垜浠氨瑕佺敤杞崲杩愮畻绗?
```cpp
class complex {
private:
    double real, imag;
public:
    complex(double r = 0, double i = 0) : real(r), imag(i) {}
    
    // 馃憞 杞崲杩愮畻绗︼細鎶?complex 杞垚 int
    operator int() {
        return static_cast<int>(real);  // 杩斿洖瀹為儴鐨勬暣鏁伴儴鍒?
    }
};

int main() {
    complex c(3.5, 5.5);
    int x = c;  // 鉁?鑷姩璋冪敤 operator int()锛寈 = 3
    cout << x << endl;  // 杈撳嚭 3
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

## 瀵硅薄鍒涘缓鐨勬湰璐?

C++ 涓垱寤哄璞″繀鐒惰Е鍙戜袱涓楠わ細

1. 涓哄璞″垎閰嶅瓨鍌ㄧ┖闂达紙鍙€変綅缃細闈欐€佸瓨鍌ㄥ尯銆佹爤銆佸爢锛?
2. 璋冪敤鏋勯€犲嚱鏁板垵濮嬪寲璇ュ瓨鍌ㄧ┖闂?

## 鍗曚釜瀵硅薄鐨刞new`/`delete`

### 鏍稿績璇硶

- 鍒涘缓锛歚new 绫诲瀷(鍒濆鍖栧櫒);`锛堟棤鍒濆鍖栧櫒鏃惰皟鐢ㄩ粯璁ゆ瀯閫犲嚱鏁帮級
- 閿€姣侊細`delete 鎸囬拡鍚?`



## 瀵硅薄鏁扮粍鐨刞new`/`delete`

### 鏍稿績璇硶

- 鍒涘缓锛歚new 绫诲瀷[鏁扮粍澶у皬];`
- 閿€姣侊細`delete [] 鎸囬拡鍚?`锛?*蹇呴』鍔燻[]`**锛屽惁鍒欏彧浼氶攢姣佺涓€涓厓绱狅紝瀵艰嚧鍐呭瓨娉勬紡锛?

### 鍏抽敭鐗规€?

1. `new[]`浼氫负鏁扮粍涓?*姣忎釜鍏冪礌鑷姩璋冪敤榛樿鏋勯€犲嚱鏁?*
2. `delete[]`浼氫负鏁扮粍涓?*姣忎釜鍏冪礌鑷姩璋冪敤鏋愭瀯鍑芥暟**
3. 浠讳綍鐢盽new`鍒涘缓鐨勫璞?/ 鏁扮粍锛屽繀椤荤敱瀵瑰簲鐨刞delete`/`delete[]`閿€姣?
4. 鍚屼竴涓寚閽?*鍙兘琚玚delete`/`delete[]`鎵ц涓€娆?*锛岄噸澶嶉噴鏀句細瀵艰嚧绋嬪簭閿欒

# 14.Inheritance & Composition

鏈儴鍒嗚璁轰唬鐮佸鐢ㄧ殑涓ょ褰㈠紡锛?

## Composition

鍦ㄦ柊绫讳腑鍒涘缓宸叉湁绫荤殑瀵硅薄

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

鍩虹被蹇呴』鍏堝畾涔夛紝涓嶇劧娌℃硶鐢?

### The constructor initializer list

鏋勯€犲嚱鏁板拰鏋愭瀯鍑芥暟涓嶄細琚户鎵匡紝璧嬪€艰繍绠椾篃涓嶄細琚户鎵?

瀛愮被鐨勬瀯閫犲嚱鏁版棤娉曡闂埗绫荤殑private

鎵€浠ユ垜浠紩鍑轰互涓嬭В鍐冲瓙绫绘瀯閫犲嚱鏁拌闂埗绫籶rivate鐨勬柟娉?

```cpp
class X {
    int a;
public:
    X(int i) : a(i) { }  // X 娌℃湁榛樿鏋勯€?
};

class Y {
    int b;
    X x;  // 鎴愬憳瀵硅薄
public:
    // 蹇呴』鐢ㄥ垵濮嬪寲鍒楄〃缁?x 浼犲弬
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
class Y: public X {  // 1. 缁ф壙锛歒鏄疿鐨勫瓙绫?
    int b;           // 鏅€氭垚鍛?
    X x1, x2;        // 2. 缁勫悎锛歒閲屽寘鍚?涓猉瀵硅薄x1銆亁2锛堝０鏄庨『搴忥細x1鍦ㄥ墠锛寈2鍦ㄥ悗锛?
public:
    // 鍒濆鍖栧垪琛細椤哄簭鏄?b(i), x2(j), x1(m), X(n)
    Y(int i, int j, int m, int n): b(i), x2(j), x1(m), X(n)
    {cout<<"Constructor Y:"<<b<<endl;}
};
int main() {
    Y y(1,2,3,4);  // 鍒涘缓Y瀵硅薄y锛屼紶鍙俰=1, j=2, m=3, n=4
    return 0;
}
Constructor X: 4  鈫?鈶?鍩虹被X瀛愬璞℃瀯閫?
Constructor X: 3  鈫?鈶?鎴愬憳瀵硅薄x1鏋勯€?
Constructor X: 2  鈫?鈶?鎴愬憳瀵硅薄x2鏋勯€?
Constructor Y: 1  鈫?鈶?Y鑷繁鐨勬瀯閫犲嚱鏁颁綋鎵ц
```

鍏堟瀯閫犲熀绫?>鎸夌収澹版槑椤哄簭鏋勯€犳垚鍛樺璞?>鏋勫缓鑷韩

## Name hiding

| 姒傚康                    | 鑻辨枃鍏ㄧО    | 鏍稿績瀹氫箟                                                     | 浣滅敤鍩?                     | 鍏抽敭鐗瑰緛                                                 |
| ----------------------- | ----------- | ------------------------------------------------------------ | --------------------------- | -------------------------------------------------------- |
| **閲嶈浇 (Overloading)**  | Overloading | 鍚屼竴浣滅敤鍩熷唴锛屽嚱鏁板悕鐩稿悓銆佸弬鏁板垪琛ㄤ笉鍚岋紙涓暟 / 绫诲瀷 / 椤哄簭锛?| **鍚屼竴涓被 / 鍚屼竴涓綔鐢ㄥ煙** | 涓嶅奖鍝嶇户鎵匡紝浠呭湪鏈被鐢熸晥锛岀紪璇戝櫒鏍规嵁鍙傛暟鑷姩鍖归厤         |
| **閲嶅畾涔?(Redefining)** | Redefining  | 缁ф壙涓紝瀛愮被閲嶅啓鐖剁被鐨?*鏅€氾紙闈炶櫄锛夋垚鍛樺嚱鏁?*               | 鐖跺瓙绫讳笉鍚屼綔鐢ㄥ煙            | 瀛愮被鍚屽悕鍑芥暟浼?*闅愯棌**鐖剁被鎵€鏈夊悓鍚嶅嚱鏁帮紝鏃犺鍙傛暟鏄惁鐩稿悓 |
| **閲嶅啓 (Overriding)**   | Overriding  | 缁ф壙涓紝瀛愮被閲嶅啓鐖剁被鐨?*铏氾紙virtual锛夋垚鍛樺嚱鏁?*              | 鐖跺瓙绫讳笉鍚屼綔鐢ㄥ煙            | 鏄鎬佺殑鍩虹锛岃姹傚嚱鏁扮鍚嶅畬鍏ㄤ竴鑷达紝涓嶄細闅愯棌鐖剁被鐗堟湰     |

```cpp
class Derived1 : public Base
{
public:
    // 鍙噸瀹氫箟浜唃()锛屽畬鍏ㄦ病纰癴()
    void g() const {}
};
class Derived2 : public Base
{
public:
    // Redefinition: 瀹屽叏閲嶅啓浜嗙埗绫荤殑鏃犲弬f()锛堢鍚嶅畬鍏ㄤ竴鑷达級
    int f() const
    {
        cout << "Derived2::f()\n";
        return 2;
    }
};
class Derived3 : public Base
{
public:
    // Change return type: 鎶婅繑鍥炲€间粠int鏀规垚void锛屽嚱鏁板悕杩樻槸f()
    void f() const
    {
        cout << "Derived3::f()\n";
    }
};
class Derived4 : public Base
{
public:
    // Change argument list: 鎶婂弬鏁颁粠鏃犲弬/string鏀规垚int锛屽嚱鏁板悕杩樻槸f()
    int f(int) const
    {
        cout << "Derived4::f()\n";
        return 4;
    }
};
```

### Choosing composition vs inheritance

![image-20260412133031660](img/oop_assets/image-20260412133031660.png)

## Access Control

| 缁ф壙鏂瑰紡                  | 娲剧敓绫诲唴閮ㄨ闂熀绫?         | 娲剧敓绫诲璞¤闂熀绫?| 鏍稿績鐗圭偣                   | 瀵瑰簲鍏崇郴                    |
| ------------------------- | --------------------------- | ------------------ | -------------------------- | --------------------------- |
| **public锛堝叕鏈夌户鎵匡級**    | 鍙闂?`public`/`protected` | 浠呭彲璁块棶 `public`  | 鍩虹被鎺ュ彛瀹屽叏鏆撮湶缁欏閮?    | **is-a锛堟槸涓€涓級**锛屾渶甯哥敤  |
| **protected锛堜繚鎶ょ户鎵匡級** | 鍙闂?`public`/`protected` | **涓嶅彲璁块棶浠讳綍**   | 鍩虹被鎺ュ彛鍙鍚庝唬娲剧敓绫诲彲瑙?| 鍑犱箮涓嶇敤                    |
| **private锛堢鏈夌户鎵匡級**   | 鍙闂?`public`/`protected` | **涓嶅彲璁块棶浠讳綍**   | 鍩虹被鎺ュ彛瀹屽叏瀵瑰闅愯棌       | **has-a锛堟湁涓€涓級**锛屾瀬灏戠敤 |

### public



### private

鏃犺浠€涔堢户鎵挎柟寮忥紝瀛愮被閮藉彲浠ヨ闂埗绫荤殑public鍜宲rotected

private淇グ绗﹀彧鏄娇寰楃埗绫荤殑protected&public鍙樻垚澶栭儴鏃犳硶璁块棶

### protected

protect淇グ绗﹀湪娌℃湁缁ф壙鐨勬椂鍊欏簲璇ヤ笌private娌℃湁浠讳綍鍖哄埆

娲剧敓绫诲唴閮ㄦ垚鍛樺彲浠ヨ闂紝浣嗗閮ㄤ笉鍙闂?

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
//杩欎釜鏃跺€欏鏋淰鏀逛负private缁ф壙锛岄偅涔堜緷鐒跺彲浠ヤ娇鐢∕ove浜嗭紱浣嗗鏋滄槸Rectangle鏀逛负private缁ф壙灏变笉鍙互浜?

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



![image-20260412162313597](img/oop_assets/image-20260412162313597.png)

閽堝浠ヤ笂缁ф壙绉佹湁鐨勪慨鏀规柟娉曪紝鎴戜滑鍦╮ectangle鐨刾ublic閮ㄥ垎鎵嬪姩鍖呰锛堣浆鍙戯級鐖剁被鐨勫嚱鏁帮細

```cpp
class Rectangle: private Location {
public:
    void InitR(int x,int y,int w,int h);
    // 鎵嬪姩杞彂鐖剁被鐨刾ublic鎺ュ彛
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

// Derived1 淇濇姢缁ф壙 Base
class Derived1 : protected Base {};

// Derived2 鍏湁缁ф壙 Derived1
class Derived2 : public Derived1 {
public:
    void fun() {
        f1(); // 鉁?ok
        f3(); // 鉁?ok
    }
};

int main() {
    Derived1 d;
    d.f1(); // 鉂?error
    d.f3(); // 鉂?error
    return 0;
}
```

## Operator overloading & inheritance

闄や簡涓婃枃鎻愬強鐨勮祴鍊艰繍绠楃锛屽叾浠栬繍绠楃閲嶈浇閮戒細琚户鎵垮埌瀛愮被

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

涓や釜鐖剁被鐨勬瀯閫犻『搴忔槸鎸夌収缁ф壙鏃剁殑椤哄簭鏉ョ殑

![image-20260412170852112](img/oop_assets/image-20260412170852112.png)

## Incremental development

### Ambiguity

褰撲竴涓被鐨勪袱涓埗绫诲惈鏈夊悓鍚嶆垚鍛樺嚱鏁扮殑鏃跺€欙紝灏变細ambiguity锛屾垜浠簲璇ヤ娇鐢╜::`

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
//杩欐椂鍊欏鏋滃湪main涓洿鎺ョ敤c.f()浼氬彂鐢焌mbiguous閿欒
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
//杩欐牱閲嶆柊瀹氫箟涓€涓嬩篃鏄彲浠ョ殑
```

涔熸湁鍙兘瀛樺湪涓嬮潰杩欑鎯呭喌锛?

![image-20260412163947448](img/oop_assets/image-20260412163947448.png)

鎴戜滑鏈変笁绉嶆柟娉曪紝涓ょ璺熷垰鎵嶇殑鍙岄噸缁ф壙涓€鑷达紝绗笁绉嶆槸virtual base class

```cpp
class D:virtual public A,public B,virtual public C{
    
};
```

杩欐牱缁ф壙灏变細琚紭鍖?

![image-20260412164114150](img/oop_assets/image-20260412164114150.png)

瀵逛簬铏氬熀绫荤殑缁ф壙锛?

It is called only once.锛堝彧琚皟鐢ㄤ竴娆★級

Only from the most derived class.锛堝彧鑳界敱 鈥滄渶娲剧敓绫烩€?璋冪敤锛?

Called before non-virtual bases.锛堝湪鏋勯€犻潪铏氬熀绫?*涔嬪墠**璋冪敤锛?

Listed in all derived classes' initializers.锛堟墍鏈夋淳鐢熺被閮藉繀椤诲垪鍑哄畠锛?

If omitted, use default constructor.锛堢渷鐣ュ垯闅愬紡璋冪敤榛樿鏋勯€狅級

![image-20260412165101504](img/oop_assets/image-20260412165101504.png)

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

璁╁熀绫荤殑鎸囬拡鍙互鎸囧悜娲剧敓绫?

```cpp
// 鍩虹被锛氫箰鍣?
class Instrument {
public:
    void play() const {} // 涔愬櫒鐨勯€氱敤鎺ュ彛锛氭紨濂?
};

// 瀛愮被锛氱涔愬櫒锛屽叕鏈夌户鎵縄nstrument锛堟弧瓒砳s-a锛氱涔愬櫒鏄竴绉嶄箰鍣級
class Wind : public Instrument {};
// 缁欎箰鍣ㄨ皟闊崇殑鍑芥暟锛屾帴鏀禝nstrument&绫诲瀷鐨勫弬鏁?
void tune(Instrument& i) { 
    i.play(); // 璋冪敤涔愬櫒鐨刾lay鏂规硶
}

```

# 15.Polymorphism & Virtual Functions

鎺ョ潃涓婇潰鐨剈pcasting 

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

杩欑鎯呭喌鐨刞Wind::play`娌℃湁鎴愬姛璋冪敤锛屽彧璋冪敤浜哷Instrumental::play`

鎴戜滑瑕佺敤铏氬嚱鏁拌В鍐宠繖涓棶棰?

## Virtual functions

The redefinition of a virtual function in a derived class is usually called overriding

Polymorphism:  Different functions with the same name. 

Virtual Functions(overriding) : dynamically determined

Function overloading: statically determined

| 鐗规€?| Function Overloading | Virtual Function |
| ---- | -------------------- | ---------------- |
| 鏃堕棿 | 缂栬瘧鏈?              | 杩愯鏃?          |
| 缁戝畾 | static binding       | dynamic binding  |
| 鏉′欢 | 鍙傛暟涓嶅悓             | 缁ф壙 + virtual   |
| 鏈川 | 鍚嶅瓧澶嶇敤             | 琛屼负澶氭€?        |

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
    virtual void play( ) const //no 鈥渧irtual鈥?is ok.
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

![image-20260412173459218](img/oop_assets/image-20260412173459218.png)

铏氬嚱鏁颁笌浠栫殑鍚屽悕鍑芥暟蹇呴』鏄悓涓€绫荤殑

**鍙湁閫氳繃鍩虹被鎸囬拡/寮曠敤鎵嶄細瑙﹀彂杩愯鏃跺鎬?*

![image-20260412173853411](img/oop_assets/image-20260412173853411.png)

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

![image-20260412174115556](img/oop_assets/image-20260412174115556.png)

## Abstract base classed and pure virtual functions

鎶借薄绫绘渶灏戞湁涓€涓函铏氬嚱鏁?

鎶借薄绫讳笉鍙疄渚嬪寲

瀵逛簬鎶借薄绫荤殑鎵€鏈夋寚閽堥兘浼歶pcasting

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

瀛愮被 VTABLE = 鍩虹被 VTABLE 鐨勨€滃鍒?+ 淇敼鈥?

濡傛灉瀛愮被override锛屽垯鏇挎崲鎴愬瓙绫荤殑鍑芥暟鍦板潃

濡傛灉瀛愮被娌verride锛屽垯娌跨敤鐖剁被鐨勫湴鍧€

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

![image-20260412203515316](img/oop_assets/image-20260412203515316.png)

```cpp
Dog(const string& petName) : Pet(petName) {}
                               鈫?
                           杩欓噷灏辨槸鍒濆鍖栧垪琛?
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

`describe(d)`鐨勬椂鍊欐垜浠殑d渚濈劧鏄痯et鐨勬柟娉曪紝鑰宒鏄痙og瀵硅薄锛岄偅涔堟垜浠氨鏃犳硶琛ㄧずdog鑷繁鐨勯儴鍒哷sleep`

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

瀵逛簬涓€涓瀯閫犺繃绋嬶紝鎴戜滑鍏堟瀯閫犱粬鐨剉ptr锛岃vptr鎸囧悜鐖剁被鐨剉table骞朵慨鏀逛负缁ф壙绫荤殑vtable

鏋勯€犲嚱鏁颁笉鑳芥槸铏氬嚱鏁帮紝鍥犱负鏋勯€犲嚱鏁拌礋璐ｆ瀯閫爒ptr

![image-20260412205641669](img/oop_assets/image-20260412205641669.png)

## Destructors and virtual destructors

- The destructor can and often must be virtual.
- If the destructor in the base is declared virtual, destructors in derived classes are all virtual even without the keyword virtual.
- To ensure destructors can be called exactly.
![image-20260412211027289](img/oop_assets/image-20260412211027289.png)

濡傛灉鍩虹被鏋愭瀯鍑芥暟鏄?virtual锛岄偅涔堟墍鏈夋淳鐢熺被鏋愭瀯鍑芥暟鑷姩涔熸槸 virtual锛堝嵆浣夸笉鍐?virtual锛?

## Operator overloading

杩愮畻绗﹂噸杞藉彲浠ュ悜鍏朵粬浠讳綍鍑芥暟涓€鏍穠irtual

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
    cout<<max (鈥榓鈥?  3)<<endl; 
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


