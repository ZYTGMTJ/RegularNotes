# 拷贝控制
**类是C++的核心概念。**第七章涵盖了使用类的所有基本知识：类作用域、数据隐藏以及构造函数，还介绍了类的一些重要特性：成员函数、隐式this指针、友元以及const、static和mutabe成员。在这一部分，将延伸类的相关话题的讨论，将介绍拷贝控制、重载运算符、继承和模版<br>

如前所述，C++中，我们通过定义构造函数来控制在类类型的对象初始化时做什么。类还可以控制在对象拷贝、赋值、移动和销毁时做什么。在这方面，C++与其他语言不同，其他很多语言都没有给予类设计者控制这些操作的能力。在拷贝控制将介绍新标准的两个重要概念：右值引用和移动操作<br>
运算符重载，这种机制允许内置运算符作用于类类型的运算对象。这样我们创建的类型直观上就可以像内置类型一样使用，运算符重载是C++借以实现这一目的的方法之一<br>
**类可以重载的运算符中有一种特殊的运算符----函数调用运算符。**对于重载了这种运算度的类，我们可以“调用”其对象，就好像它们是函数一样。新标准库中提供了一些设施，使得不同类型的可调用对象可以以一种一致的方式来使用。在类型转换章节中将介绍一种特殊类型的类成员函数----转换运算符。这些运算符定义了类类型对象的隐式转换机制。编译器应用这种转换机制的场合与原因都与内置类型转换是一样的。<br>


这一章的内容非常重要，要好好看！
本章中我们将学习类如何控制该类型对象拷贝、赋值、移动或销毁时做什么。类通过一些特殊的成员函数控制这些操作，包括：**拷贝构造函数、移动构造函数、拷贝赋值运算符、移动赋值运算符以及析构函数。**<br>
当定义一个类时，我们显示地或隐式地指定在此类型的对象拷贝、移动、赋值和销毁时做什么。一个类通过定义五种特殊的成员函数来控制这些操作，包括：拷贝构造函数、拷贝赋值运算符、移动构造函数、移动赋值运算符和析构函数。<br>
@拷贝和移动构造函数定义了当同类型的另一个对象初始化本对象时做什么。<br>
@拷贝和移动赋值运算符定义了将一个对象赋予同类型的另一个对象时做什么。<br>
@析构函数定义了当此类型对象销毁时做什么。<br>
﻿我们称这些操作为拷贝控制操作<br>
@如果一个类没有定义所有这些拷贝控制成员，编译器会自动为它定义缺失的操作。因此，很多类会忽略这些拷贝控制操作。但是，对一些类来说，依赖这些操作默认定义会导致灾难。通常，实现拷贝控制操作最困难的地方是首先认识到什么时候需要定义这些操作。<br>
## 拷贝、赋值与销毁
拷贝构造函数、拷贝赋值运算符和析构函数作为开始
## 拷贝构造函数
@如果一个构造函数的第一个参数是自身类型的引用，且任何额外参数都有默认值，则此构造函数是拷贝构造函数
```cpp
class Foo{
public:
    Foo(); //默认构造函数 
    Foo(const Foo&); //拷贝构造函数
    //...
};
```
拷贝构造函数的第一个参数必须是一个引用类型！拷贝构造函数在几种情况下都会被隐式地使用。因此，拷贝构造函数通常不应该是explicit<br>
**合成拷贝构造函数**
﻿如果我们没有为一个类定义拷贝构造函数，编译器会为我们定义一个。与合成默认构造函数不同，即使我们定义了其他构造函数，编译器也会为我们合成一个拷贝构造函数。<br>
@合成拷贝构造函数是缺省的拷贝构造函数<br>
@合成拷贝构造函数就等同于对所有成员变量进行一次赋值操作<br>
@对于基础类，都最好实现构造函数，拷贝构造函数，析构函数<br>
对某些类来说，合成拷贝构造函数用来阻止我们拷贝该类类型的对象。而一般情况，合成的拷贝构造函数会将其参数的成员逐个拷贝到正在创建的对象中。编译器从给定对象中依次将每个非static成员拷贝到正在创建的对象中。<br>
每个成员的类型决定了它如何拷贝：
@对类类型的成员，会使用其拷贝构造函数来拷贝；<br>
@内置类型的成员则直接拷贝。虽然我们不能直接拷贝一个数组，但合成拷贝构造函数会逐元素地拷贝一个数组类型的成员。如果数组元素是类类型，则使用元素的拷贝构造函数来进行拷贝。<br>
举个：
```cpp
class Sales_data{
public:
    Sales_data(const Sales_data&); //其他成员和构造函数的定义，如前，与合成的拷贝构造函数等价的拷贝构造函数的声明
private:
    std::String bookNo;
    int units_sold = 0;
    double revenue = 0.0;
};
//与Sales_data 的合成的拷贝构造函数等价
Sales_data::Sales_data(const Sales_data &orig):
bookNo(orig.bookNo),  //使用string的拷贝构造函数
units_sold(orig.units_sold), //拷贝orig.units_sold
revenue(orig.revenue){} //拷贝orig.revenue
```
## 拷贝构造函数
```cpp
string dots(10,'.'); //直接初始化
string s(dots);  //直接初始化
string s2 = dots; //拷贝初始化
string nines = string(100,'9'); //拷贝初始化
```
@当使用直接初始化时，我们实际上是要求编译器使用普通的函数匹配来选择与我们提供的参数最匹配的构造函数。<br>
@当我们使用拷贝初始化时，我们是要求编译器将右侧运算对象拷贝到正在创建的对象中，如果需要的话还要进行类型转换。<br>
@拷贝初始化通常使用拷贝构造函数来完成。但是，如果一个类有一个移动构造函数，则拷贝初始化有时会使用移动构造函数而非拷贝构造函数来完成。<br>
**拷贝构造函数发生的情况**
@用=定义变量时会发生<br>
@将一个对象作为实参传递给一个非引用类型的形参<br>
@从一个返回类型为非引用类型的函数返回一个对象<br>
@用花括号列表初始化一个数组中的元素或一个聚合类中的成员<br>
**但凡涉及赋值，都用拷贝构造函数完成**<br>
@某些类类型还会对它们所分配的对象使用拷贝初始化。例如当我们初始化标准库容器或是调用其insert或push成员时，容器会对其元素进行拷贝初始化。与之相对，用emplace成员创建的元素都进行直接初始化。<br>
## 参数和返回值
在函数调用过程中，具有非引用类型的参数要进行拷贝初始化。类似的，当一个函数具有非引用的返回类型时，返回值会被用来初始化调用方的结果。<br>
拷贝构造函数被用来初始化非引用类类型参数。为了调用拷贝构造函数，我们必须拷贝它的实参，但为了拷贝实参，我们有需要掉哟个拷贝构造函数，如此无限循环。<br>
## 拷贝初始化的限制
如前所述，如果我们使用的初始化值要求通过一个explicit的构造函数来进行类型转换，那么使用拷贝初始化还是直接初始化就不是无关紧要的了<br>
**编译器可以绕过拷贝构造函数**<br>
在拷贝初始化过程中，编译器可以（但不是必须的）跳过拷贝/移动构造函数，直接创建对象。即，编译器被允许<br>
```cpp
string null_oo("0000000");
class Hasptr{
public:
    HasPtr(const string &s = string()):ps(new string(s)),i(0){}
private:
    string *ps;
    int i;
};

HasPtr::HasPtr(const HasPtr &ptr):ps(new string(*(ptr.ps))), i(ptr.i){}
```
## 拷贝赋值运算符
与控制其对象如何初始化一样，类也可以控制其对象如何赋值：<br>
```cpp
Sales_data trans,accum;
trans = accum; // 使用Sales_data的拷贝赋值运算符
```
与拷贝构造函数一样，如果类未定义自己的拷贝赋值运算符，编译器会为它合成一个<br>
## 重载赋值运算符
重载运算符本质上是函数，其名字由operator 关键字后接表示要定义的运算符符号组成。因此，赋值运算符就是一个名为operator=的函数。类似于任何其他函数，运算符函数也有一个返回类型和一个参数列表<br>
重载运算符的参数表示运算符的运算对象。某些运算符，包括赋值运算符，必须定义为成员函数。如果一个运算符是一个成员函数，其左侧运算对象就绑定到隐式的this参数。对于一个二元运算符。例如赋值运算符，其右侧运算对象作为显式参数传递。<br>
拷贝赋值运算符接受一个与其所在类相同类型的参数：<br>
```cpp
class Foo{
public:
Foo& operator=(const Foo&); //赋值运算符
//...
};
```
为了与内置类型的赋值保持一致，赋值运算符通常返回一个指向其左侧运算对象的引用。另外值得注意的是，标准库通常要求保存在容器中的类型要具有赋值运算符，且其返回值是左侧运算对象的引用。<br>
## 合成拷贝赋值运算符
与处理拷贝构造函数一样，如果一个类未定义自己的拷贝赋值运算符，编译器会为它生成一个合成拷贝赋值运算符。类似拷贝构造函数，对于某些类，合成拷贝赋值运算符用来禁止该类型对象的赋值。如果拷贝赋值运算符并非出于此目的，它会将右侧运算对象的每个非static成员赋予左侧运算对象的对应成员，这一工作是通过成员类型拷贝赋值运算符来完成的。对于数组类型的成员，逐个赋值数组元素。合成拷贝赋值运算符返回一个指向其左侧运算对象的引用。<br>
举个：
```cpp
Sales_data& Sales_data::operator=(const Sales_data &rhs){
bookNo = rhs.bookNo; //调用string::operator=
units_sold = rhs.units_sold; //使用内置的int赋值
revenue = rhs.revenue; //使用内置的double赋值
return *this; //返回一个此对象的引用
}
```
❓拷贝赋值运算符是什么 ：是一个名为operator=的函数<br>
❓什么时候使用它 ：当赋值运算发生时就会用到它<br>
❓合成拷贝赋值运算符完成什么工作：可以用来禁止该类型对象的赋值<br>
❓什么时候会生成合成拷贝赋值运算符：如果一个类为定义自己的拷贝赋值运算符，编译器会为它生成一个合成拷贝赋值运算符<br>
为HasPtr编写赋值运算符。类似拷贝构造函数，赋值运算符应该将对象拷贝到ps指向的位置<br>

```cpp
#ifndef ex13_8_h
#define ex13_8_h

#include <string>
class HasPtr{
public:
    HasPtr(const string& s = string()):ps(new string()),i(0){}
    HasPtr(const HasPtr& hp):ps(new string(*hp.ps)),i(hp.i){}
    HasPtr& operator=(const HasPtr& hp){
        string *new_ps = new string(*hp.ps);
        delete ps;
        ps = new_ps;
        i = hp.i;
        return *this;
}
private:
    string *ps;
    int i;
};
#endif
```
## 析构函数
析构函数执行与构造函数相反的操作：@构造函数初始化对象的非static数据成员，还可能做一些其他工作；<br>
@析构函数释放对象使用的资源，并销毁对象的非static数据成员。<br>
析构函数是类的一个成员函数，名字由波浪号接类名构成。他没有返回值，也不接受参数：<br>
```cpp
class Foo{
public:
    ~Foo(); //
};
```
由于析构桉树不接受参数，因此它不能被重载。对一个给定类，只会有唯一一个析构函数<br>
析构函数完成什么工作<br>
如同构造函数有一个初始化部分和一个函数题，析构函数也有一个函数体和一个析构部分。在一个构造函数中，成员的初始化是在函数体执行之前完成的，且按照他们在类中出现的顺序进行初始化。在一个析构函数中，首先执行函数体，然后销毁成员。成员按初始化顺序的逆序销毁<br>
在对象最后一次使用之后，析构函数体可执行类设计者希望执行的任何收尾工作。通常，析构函数释放对象在生存期分配的所有资源<br>
在一个析构函数中，不存在类似构造函数中初始化列表的东西来控制成员如何销毁，析构部分是隐式的。成员销毁时发生什么完全依赖于成员的类型。@销毁类类型的成员需要执行成员自己的析构函数。内置类型没有析构函数，因此销毁内置类型成员什么都不需要做<br>
隐式销毁一个内置指针类型的成员不会delete它所指向的对象<br>
与普通指针不同，智能指针是类类型，所以具有析构函数<br>
因此，与普通指针不同，智能指针成员在析构阶段会被自动销毁<br>
**什么时候会调用析构函数**
@变量在离开其作用域时被销毁<br>
@当一个对象被销毁时，其成员被销毁<br>
@容器（无论是标准库容器还是数组）被销毁时。其元素被销毁<br>
@对于动态分配的对象，当对指向它的指针应用delete运算符时被销毁<br>
@对于临时对象，当创建它的完整表达式结束时被销毁<br>
由于析构函数自动运行，我们的程序可以按需要分配资源，而（通常）无须担心何时释放这些资源<br>
```cpp
{//新作用域
Sales_data *p = new Sales_data; //p是一个内置指针
auto p2 = make_shared<Sales_data>(); //p2是一个shared_ptr
Sales_data item(*p); //拷贝构造函数将*p拷贝到item中
vector<Sales_data> vec; //局部对象
vec.push_back(*p2); //拷贝p2指向的对象
delete p; //对p指向的对象执行析构函数
} //退出局部作用域；对item、p2和vec调用析构函数
//销毁p2会递减其引用计数；如果引用计数变为0，对象被释放
//销毁vec会销毁它的元素
```
其他Sales_data 对象会在离开作用域时被自动销毁。shared_ptr的析构函数会递减p2指向的对象的引用计数。<br>
在所有情况下，Sales_data的析构函数都会隐式地销毁bookNo成员。销毁bookNo会调用string的析构函数，它会释放用来保存ISBN的内存<br>
@当指向一个对象的引用或指针离开作用域时，**析构函数不会执行**<br>
## 合成析构函数
当一个类未定义自己的析构函数时，编译器会为它定义一个合成析构函数。类似拷贝构造函数和拷贝赋值运算符，对于某些类，合成析构函数被用来阻止改类型的对象被销毁。如果不是这种情况，合成析构函数的函数体为空<br>
```cpp
class Sales_data{
public:
~Sales_data(){}
};
```
@认识到析构函数体自身并不直接销毁成员是非常重要的。成员在析构函数体之后隐含的析构阶段中被销毁。在整个对象销毁过程中，析构函数体是作为成员销毁步骤之外的另一部分而进行的<br>
## 三/五法则
三个基本操作可以控制类的拷贝操作：拷贝构造函数、拷贝赋值运算符和析构函数<br>
**第一：需要析构函数的类也需要拷贝和赋值操作**<br>
@当我们决定一个类是否要定义它自己版本的拷贝控制成员时，一个基本原则是首先确定这个类是否需要一个析构函数。<br>
如果一个类需要自定义析构函数，几乎可以肯定它也需要自定义拷贝赋值运算符和拷贝构造函数<br>
**第二：需要拷贝操作的类也需要赋值操作，反之亦然（无论是需要拷贝构造函数还是需要拷贝赋值运算符都不必然意味着也需要析构函数）**

假定numbered是一个类，它有一个默认构造函数，能为每个对象生成一个唯一的序号，保存在名为mysn的数据成员中。假定numbered使用合成的拷贝控制成员，并给定函数：<br>
```cpp
void f(numbered s) { cout<< s.mysn <<endl; }
```
则下面代码输出什么内容？<br>
numbered a,b = a, c=b;
f(a);f(b);f(c);
输出三个一样的数 //对所有成员进行一次赋值操作<br>
假定numbered定义了一个拷贝构造函数，能生成一个新的序列号。这会改变上一题中调用的输出结果嘛？如果改变，为什么？新的输出是什么<br>
会！三个不同的数，并不是a,b,c中的数<br>
如果f中的参数是const numbered&,将会怎样？这会改变输出的结果嘛？如果会改变，为什么？新的输出结果是什么<br>
输出a,b,c<br>
## 使用=default
我们可以通过将拷贝控制成员定义为=default来显式地要求编译器生成合成的版本<br>
```cpp
class Sales_data{
public:
Sales_data() = default;
Sales_data(const Sales_data&) = default;
Sales_data& operator=(const Sales_data &);
~Sales_data() = default;
};

Sales_data& Sales_data::operator=(const Sales_data&) = default;
```
当我们在类内用=default修饰成员的声明时，合成的函数将隐式地声明为内敛的。如果我们不希望合成的成员是内联函数，应该只对成员的类外定义用=default，就像对拷贝赋值运算符所做的那样<br>
## 阻止拷贝
@大多数类应该定义默认构造函数、拷贝构造函数和拷贝赋值运算符，无论是隐式地还是显式地<br>
## 定义删除的函数
在新标准下，我们可以通过将拷贝构造函数和拷贝赋值运算符定义为删除的函数来阻止拷贝。删除的函数是这样一种函数：我们虽然声明了他们，但不能以任何方式使用它们。在函数的参数列表后面加上=delete来指出我们希望将它定义为删除的：<br>
```cpp
struct noCopy{
    NoCopy() = default; //使用合成的默认构造函数
    NoCopy(const NoCopy&) = delete; //阻止拷贝
    NoCopy &operator=(const NoCopy&) = delete; //阻止赋值
    ～NoCopy() = default; // 使用合成的析构函数
};
```
**析构函数不能是删除的成员**
⚠️对于析构函数已删除的类型，不能定义该类型的变量或释放指向该类型动态分配对象的指针<br>
**合成的拷贝控制成员可能是删除的。**<br>
如前所述，如果我们未定义拷贝控制成员，编译器会为我们定义合成的版本。类似的，如果一个类为定义构造函数，编译器会为其合成一个默认构造函数。对某些类来说，编译器将这些合成的成员定义为删除的函数：<br>
@如果类的某个成员的析构函数是删除的或不可访问的，则类的合成析构函数被定义为删除的<br>
@如果类的某个成员的拷贝构造函数是删除的或不可访问的，则类的合成拷贝构造函数被定义为删除的。如果类的某个成员的析构函数是删除的或不可访问的，则类合成的拷贝构造函数也被定义为删除的<br>
@如果类的某个成员的拷贝赋值运算符是删除的或不可访问的，或是类有一个const的或引用成员，则类的合成拷贝赋值运算符被定义为删除的<br>
@如果类的某个成员的析构函数是删除的或不可访问的，或是类有一个引用成员，它没有类内初始化器，或是类有一个const成员，他们有类内初始化器且其类型为显式定义默认构造函数，则该类的默认构造函数被定义为删除的<br>
这些规则的含义：如果一个类有数据成员不能默认构造、拷贝、赋值或销毁，则对应的成员函数将被定义为删除的<br>
本质上，当不可能拷贝、赋值或销毁类成员时，类的合成拷贝控制成员就被定义为删除的<br>
## private拷贝控制
在新标准发布之前，类是通过将其拷贝构造函数和拷贝赋值运算符声明为private的来阻止拷贝<br>
```cpp
class PrivateCopy{
PrivateCopy(const PrivateCopy&);
PrivataCopy &operator=(const PrivateCopy&);
public:
PrivateCopy() = default;//使用合成的默认构造函数
~PrivateCopy(); // 用户可以定义此类型的对象，但是无法拷贝它
```
希望阻止拷贝的类应该使用=delete来定义它们自己的拷贝构造函数和拷贝赋值运算符，而不应该将它们声明为private<br>
❓定义一个Employee类，它包含雇员的姓名和唯一的雇员证号。为这个类定义默认构造函数，以及接受一个表示雇员姓名的string的构造函数。每个构造函数应该通过递增一个static数据成员来生成一个唯一的证号<br>
```cpp
class Employee{
public:
    Employee();
    Employee(const string& name);
    Employee(const Employee&) = delete;
    Employee& operator=(const Employee&)=delete;
    const int id() const{ return id_;}
private:
    string name_;
    int id_;
    static int s_increment;
};

int Employee::s_increment = 0;
Employee::Employee(){
    id_ = s_increment++;
}
Employee::Employee(const string& name){
    id_ = s_increment++;
    name_ = name;
}
#endif
```

## 拷贝控制和资源管理
通常，管理类外资源的类必须定义拷贝控制成员。<br>
为了定义拷贝构造函数，拷贝赋值运算符以及析构函数，这些成员，我们首先必须确定此类型对象的拷贝语义。一般来说，有两种选择：可以定义拷贝操作，使类的行为看起来像一个值或者像一个指针<br>
@类的行为像一个值，意味着它应该也有自己的状态。当我们拷贝一个像值的对象时，副本和愿对象是完全独立的。改变副本不会对原对象有任何影响，反之亦然。<br>
@行为像指针的类则共享状态。当我们拷贝一个这种类的对象时，副本和原对象使用相同的底层数据。改变副本也会改变原对象，反之亦然<br>
```cpp
class HasPtr{
public:
HasPtr(const std::string &s = std::string()):ps(new std::string(s)),i(0){}
HasPtr(const HasPtr&):ps(new string(*hp.ps)),i(hp.i){};
HasPtr& operator=(const HasPtr &hp){
auto new_p = new string(*hp.ps);
delete ps;
ps = new_p;
i = hp.i;
return *this;
}
~HasPtr(){
delete ps;
}
private:
string *ps;
int i;
};
```

## 行为像值的类
为了提供类值的行为，对于类管理的资源，每个对象都应该拥有一份自己的拷贝。这意味着对于ps指向的string，每个HasPtr对象都必须有自己的拷贝。为了实现类值的行为，HasPtr需要<br>
@定义一个拷贝构造函数，完成string的拷贝，而不是拷贝指针
@定义一个析构函数来释放string
@定义一个拷贝赋值运算符来释放对象当前的string。并从右侧运算对象拷贝string
**类值版本**
```cpp
class HasPtr{
    public:
        HasPtr(const string &s = string()):ps(new string(s)),i(0){} //对ps指向的string，每个HasPtr对象都有自己的拷贝
        HasPtr(const HasPtr &p):ps(new string(*p.ps)),i(p.i){}
        HasPtr& operator=(const HasPtr &);
        ~HasPtr(){delete ps;}
    private:
        string *ps;
        int i;
}
```
我们的类足够简单，在类内就已定义了除赋值运算符之外的所有成员函数。第一个构造函数接受一个（可选的）string参数。
**类值拷贝赋值运算符**
赋值运算符通常组合了析构函数和构造函数的操作。<br>
类似析构函数，赋值操作会销毁左侧运算对象的资源。<br>
类似拷贝构造函数，赋值操作会从右侧运算对象拷贝数据<br>
*非常重要一点*
这些操作是以正确的顺序执行的，即使将一个对象赋予它自身，也保证正确。而且，如果可能，我们编写的赋值运算符还应该是异常安全的---当异常发生时，能将左侧运算对象置于一个有意义的状态<br>
举个🌰
通过先拷贝右侧运算对象，我们可以处理自赋值情况，并能保证在异常发生时代码也是安全的。在完成拷贝后，我们释放左侧运算对象的资源，并更新指针指向新分配的string<br>
```cpp
HasPtr& HasPtr::operator=(const HasPtr &rhs){
    auto newp = new string(*rhs.ps); //拷贝底层string
    delete ps; //释放旧内存
    ps = newp; //从右侧运算对象拷贝数据到本对象
    i = rhs.i;
    return *this; //返回本对象
}
```
*在这个赋值运算符中，非常清楚，我们首先进行了构造函数的工作：newp的初始化器等价于HasPtr的拷贝构造函数中的ps的初始化器。接下来与析构函数一样，我们delete当前ps指向的string。然后就只剩下拷贝指向新分配的string的指针，以及从rhs拷贝int值到本对象了*
**key Concept**
当你编写赋值运算对象时，两点要记住<br>
@ 如果将一个对象赋予他自身，赋值运算符必须能正确工作<br>
@ 大多数赋值运算符组合了析构函数和拷贝构造函数的工作<br>
一个好的模式是先将右侧运算对象拷贝到一个局部临时对象中。当拷贝完成后，销毁左侧运算对象的现有成员就是安全的了。一旦左侧运算对象的资源被销毁，就只剩下将数据从临时对象拷贝到左侧运算对象的成员中了<br>
❓若本节的HasPtr版本为定义析构函数，将会发生什么？若未定义拷贝构造函数，将会发生什么？<br>
*未定义析构函数，将会发生内存泄漏。 若未定义拷贝构造函数，将会拷贝指针的值，指向同一个地址*
❓假定希望定义 StrBlob 的类值版本，而且希望继续使用 shared_ptr，这样我们的 StrBlobPtr 类就仍能使用指向vector的 weak_ptr 了。你修改后的类将需要一个拷贝的构造函数和一个拷贝赋值运算符，但不需要析构函数。解释拷贝构造函数和拷贝赋值运算符必须要做什么。解释为什么不需要析构函数<br>
*拷贝构造函数和拷贝赋值运算符要重新动态分配内存。因为 StrBlob 使用的是智能指针，当引用计数为0时会自动释放对象，因此不需要析构函数。*
```cpp
#ifndef ex13_26_h
#define ex13_26_h

#include <vector>
#include <string>
#include <initializer_list>
#include <memory>
#include <exception>

using std::vector; using std::string;

class ConstStrBlobPtr;

class StrBlob
{
public:
	using size_type = vector<string>::size_type;
	friend class ConstStrBlobPtr;

	ConstStrBlobPtr begin() const;
	ConstStrBlobPtr end() const;

	StrBlob() :data(std::make_shared<vector<string>>()) {}
	StrBlob(std::initializer_list<string> il) :data(std::make_shared<vector<string>>(il)) {}

	// copy constructor
	StrBlob(const StrBlob& sb) : data(std::make_shared<vector<string>>(*sb.data)) {}
	// copyassignment operators
	StrBlob& operator=(const StrBlob& sb);

	size_type size() const { return data->size(); }
	bool empty() const { return data->empty(); }

	void push_back(const string &t) { data->push_back(t); }
	void pop_back()
	{
		check(0, "pop_back on empty StrBlob");
		data->pop_back();
	}

	std::string& front()
	{
		check(0, "front on empty StrBlob");
		return data->front();
	}

	std::string& back()
	{
		check(0, "back on empty StrBlob");
		return data->back();
	}

	const std::string& front() const
	{
		check(0, "front on empty StrBlob");
		return data->front();
	}
	const std::string& back() const
	{
		check(0, "back on empty StrBlob");
		return data->back();
	}

private:
	void check(size_type i, const string &msg) const
	{
		if (i >= data->size()) throw std::out_of_range(msg);
	}

private:
	std::shared_ptr<vector<string>> data;
};

class ConstStrBlobPtr
{
public:
	ConstStrBlobPtr() :curr(0) {}
	ConstStrBlobPtr(const StrBlob &a, size_t sz = 0) :wptr(a.data), curr(sz) {} // should add const
	bool operator!=(ConstStrBlobPtr& p) { return p.curr != curr; }
	const string& deref() const
	{ // return value should add const
		auto p = check(curr, "dereference past end");
		return (*p)[curr];
	}
	ConstStrBlobPtr& incr()
	{
		check(curr, "increment past end of StrBlobPtr");
		++curr;
		return *this;
	}

private:
	std::shared_ptr<vector<string>> check(size_t i, const string &msg) const
	{
		auto ret = wptr.lock();
		if (!ret) throw std::runtime_error("unbound StrBlobPtr");
		if (i >= ret->size()) throw std::out_of_range(msg);
		return ret;
	}
	std::weak_ptr<vector<string>> wptr;
	size_t curr;
};

ConstStrBlobPtr StrBlob::begin() const // should add const
{
	return ConstStrBlobPtr(*this);
}
ConstStrBlobPtr StrBlob::end() const // should add const
{
	return ConstStrBlobPtr(*this, data->size());
}

StrBlob& StrBlob::operator=(const StrBlob& sb)
{
	data = std::make_shared<vector<string>>(*sb.data);
	return *this;
}


#endif // !ex13_26_h
```
**定义行为像指针的类**
...