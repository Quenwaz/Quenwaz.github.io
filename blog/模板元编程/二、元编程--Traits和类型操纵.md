---
title: 二、元编程--Traits和类型操纵
date: 2018-03-08 13:13:37
categories: 编码
tags:
  - CppMetaProgramming
  - C++
---

### 类型关联
在C++中，可以在编译期进行操纵的实体称为**元数据(metadata)**,元数据可分为(任何元数据都可以作为模板参数):
- 非类型: 
  在{% post_link 一、开始进行元编程 开始进行元编程 %}中的常量整数值就属于一种非类型，此类包括可在编译期知道的任何值类型。如：整形、枚举、函数和“全局”对象的指针与引用，及指向成员的指针。
  
- 类型:
  而类型通常需要利用关键字typename进行声明。对于typename的使用条件：当使用一个依赖性的名字(dependent name)且该名字表示的是一个类型时，C++标准要求使用typename关键字标明。

下面写一个简单的算法: iter_swap,它的主要职责用于接收两个迭代器交换它们的值：
```C++
template <class ForwardIterator1, class ForwardIterator2 i2>
void iter_swap(ForwardIterator1 i1, ForwardIterator2 i2)
{
  T tmp = *i1;
  *i1 = *i2;
  *i2 = tmp;
};
```
以上代码无法通过编译， 错误很明显，对于类型T未定义。那么如何给这个类型命名呢？请往下看。

#### 一种直接的方式
要求迭代器提供一个名称为value_type的嵌套类型，可以直接访问它：
```C++
template <class ForwardIterator1, class ForwardIterator2 i2>
void iter_swap(ForwardIterator1 i1, ForwardIterator2 i2)
{
  typename ForwardIterator1::value_type tmp = *i1;
  *i1 = *i2;
  *i2 = tmp;
};
```
对于类型关联来说，这是个好的策略，但它不具有一般性。C++中迭代器的设计是模仿指针，这样做的用意是普通的指针也可以用做合法的迭代器。而指针没有嵌套类型，所以不适用。只能适用于类类型。

#### 一种迂回的方式
我们无法为所有的迭代器都添加嵌套类型::value_type，但是我们可以将其添加到一个带有迭代器类型的参数模板。在标准库中，称为iterator_traits,如下:
```C++
template <class Iterator> struct iterator_traits;
// 下面调整iter_swap:
template <class ForwardIterator1, class ForwardIterator2 i2>
void iter_swap(ForwardIterator1 i1, ForwardIterator2 i2)
{
  typename iterator_traits<ForwardIterator1>::value_type tmp = *i1;
  *i1 = *i2;
  *i2 = tmp;
};
```
iterator_traits描述了实参的特性。这里的特性（traits）是迭代器的五个相关联类型：value_type、reference、pointer、difference_type及iterator_category。

traits模板最重要的功能是提供了一种非侵入的方式，使信息与类型进行关联。你只需要添加一个iterator_traits的显式特化(explicit specialization),当iter_swap使用时，就能看到特化中的value_type类型:
```C++
template<>
struct iterator_traits<Hector::hands_off>
{
  typedef int value_type;
  //..
  // 另外四个typedefs
};
```
这种非侵入的方式，可以适用于指针，而非局限于类类型，只需局部特化一个版本：
```C++
template<class T>
struct iterator_trait<T*>
{
  typedef T value_type;
  // ...
  // 另外四个typedefs
};
```

####  寻找一种捷径
直接上代码:
```C++
template <class Iterator>
struct iterator_trait
{
  typedef typename Iterator::value_type value_type;
  // .. 另外四个typedefs
};
```
由代码可以看出，value_type 的具体类型由实参Iterator控制，作者在编码时只需要在Iterator内部添加一个value_type实际类型即可。

那么这样做的好处在哪里？相比进行特化而言，此方式明显更简洁，更能控制无必要的冗余代码。相对在类外部新增特化版本，在类内部控制更为便利。

### 元函数
相信阅读至此， 你已经注意到traits模板和普通函数之间存在某些相似性。traits的模板参数和嵌套类型在编译期发挥了类似于普通函数的参数和返回值在运行期发挥的作用。

处理传递和返回的是类型而非值外，traits模板还有两个显著的特征，他们在普通函数身上是看不到的：
- **特化**：只需要添加一个特化就可以针对其参数特定的“值”非侵入性地修改traits模板结果。或通过使用局部特化为整个范围的“值”修改结果。如果将特化应用于普通函数，将不可思议。假如添加一个重载的std::abs，只有当其实参为一个奇数的时候才能调用该特化版本。
- **多个返回值**：通常普通函数传递多个实参只返回一个值（输出实参除外）。而traits通常具有不止一个返回结果。如， std::iterator_traits包含五个嵌套类型:value_type、reference、pointer、different_type及iterator_category。

为了深入了解"类模板用作函数"的思想，我们将使用术语**元函数(metafunctions)**。

标准库中traits模板都遵从"多个返回值"模型。通常此类模型称为"blob"，因为它似乎是将单独的，松散相关的元函数揉碎后放入单个单元中。这种方式会存在如下弊端:
- 当首次进入iterator_traits访问::value_type时，该模板会被实例化，意味着编译器需要承担很多工作，而且不仅只计算value_type，其他四个相关联类型都需要计算(即使用不到)
- 降低了互操作能力，因为将多个元函数捆绑，如:
```C++
template <class X, class UnaryOp1, class UnaryOp2>
X apply_fg(X x, UnaryOp1 f, UnaryOp2 g)
{
  return f(g(x));
}
```
而利用blob的方式:
```C++
template <class X, class Blob>
X apply_fg(X x, Blob blob)
{
  return blob.f(blob.g(x));
}
```
显然第二种方式意味着必须是Blob成员，而第一种方式可以如下操作:
```C++
#include <functional>
float log2(float);

int a = apply_fg(5.0f, std::negate<float>(), log2);
int b = apply_fg(-2.14f, log2, std::negate<float>());
```
这种允许不同实参类型可交换使用的属性，成为多态(polymorphism)
> **多态**<br/>
 C++中有两种多态。常见的**动态多态(dynamic polymorphism)** 即单个基类指针或引用可处理多个派生类型的对象。而此处提到的是**静态多态(static polymorphism)** 允许不同类型的对象以同样的方式被操纵。<br/>
 - 动态多态，连同"迟绑定"或"运行期派发"(C++ 借助于虚函数提供的特性)，是面向对象编程的关键特性。通常在运行期。
 - 静态多态，也称为参数化多态是泛型编程的本质要素。通常在编译期间

### 数值元函数
数值元函数也称常量外覆器
```C++
struct five
{
  static int const value = 5;
  typedef int value_type;
  // ...
}
```
以上代码看似不方便，这样做的原因:要求所有元函数和返回类型，可以使他们更统一，更多态。

在boost所有数值型原函数都是这么做的，后续章节将讲到它所带来的好处。

### 在编译期作出选择
#### 进一步讨论iter_swap
- [ ] 待完善
<br/>
<small>*本文为学习整理《C++ Template Metaprogramming》一书中的要点*</small>

---
<small><font color= "gray">本文[Quenwaz](http://quenwaz.github.io)版权所有，转载请说明出处</font></small>