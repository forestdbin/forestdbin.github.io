---
title: The C Programming Language - annotation, 6
tags:
  - c
  - language
  - programming
categories:
  - - c
    - language
  - - language
    - c
date: 2024-10-29 22:11:35
---

## Struct

结构是一个或多个变量的集合，这些变量可能为不同的类型，为了处理的方便而将这些变量组织在一个名字之下。由于结构将一组相关的变量看作一个单元而不是各自独立的实体，因此结构有助于组织复杂的数据，特别是在大型的程序中。

ANSI标准在结构方面最主要的变化是定义了结构的赋值操作——结构可以拷贝、赋值、传递给函数，函数也可以返回结构类型的返回值。多年以前，这一操作就已经被大多数的编译器所支持，但是，直到这一标准才对其属性进行了精确定义。在ANSI标准中，自动结构和数组现在也可以进行初始化。

## 1. 结构的基本知识

关键字`struct`引入结构声明。结构声明由包含在花括号内的一系列声明组成。关键字`struct`后面的名字是可选的，称为**结构标记**。结构标记用于为结构命名。在定义之后，结构标记就代表花括号内的声明，可以用它作为该声明的简写形式。

结构中定义的变量称为**成员**。结构成员、结构标记和普通变量（即非成员）可以采用相同的名字，它们之间不会冲突，因为通过上下文分析总可以对它们进行区分。另外，不同结构中的成员可以使用相同的名字，但是，从编程风格方面来说，通常只有密切相关的对象才会使用相同的名字。

`struct`声明定义了一种数据结构。在标志结构成员表结束的右花括号之后可以跟一个变量表，这与其他基本类型的变量声明是相同的。

如果结构声明的后面不带变量表，则不需要为它分配存储空间，它仅仅描述了一个结构的模板或轮廓。但是，如果结构声明中带有标记，那么在以后定义结构实例时可以使用该标记定义。结构的初始化可以在定义的后面使用初值表进行。初值表中同每个成员对应的初值必须是常量表达式。自动结构也可以通过赋值初始化，还可以通过调用返回相应类型结构的函数进行初始化。

在表达式中，可以通过下列形式引用某个特定结构中的成员：**结构名.成员**。其中的结构成员运算符`.`将结构名与成员名连接起来。

结构可以嵌套。

## 2. 结构与函数

结构的合法操作只有几种：作为一个整体复制和赋值，通过`&`运算符取地址，访问其成员。其中，复制和赋值包括向函数传递参数以及从函数返回值。结构之间不可以进行比较。可以用一个常量成员值列表初始化结构，自动结构也可以通过赋值进行初始化。

至少可以通过3种可能的方法传递结构：一是分别传递各个结构成员，而是传递整个结构，三是传递指向结构的指针。这3种方法各有利弊。

注意，参数名和结构成员同名不会引起冲突。事实上，使用重名可以强调两者之间的关系。

...结构类型的参数和其他类型的参数一样，都是通过值传递的。

如果传递给函数的结构很大，使用指针方式的效率通常比复制整个结构的效率要高。...结构成员运算符`.`的优先级比`*`的优先级高。

结构指针的使用频度非常高，为了使用方便，C语言提供了另一种简写方式。假定p是一个指向结构的指针，可以用 **p->结构成员** 这种形式引用相应的结构成员。

运算符`.`和`->`都是从左至右结合的。

## 3. 结构数组

C语言提供了一个编译时（compile-time）一元运算符`sizeof`，它可用来计算任一对象的长度。表达式 **sizeof 对象** 以及 **sizeof(类型名)** 将返回一个整型值，它等于指定对象或类型占用的存储空间字节数。（严格地说，`sizeof`的返回值是无符号整型值，其类型为`size_t`，该类型在头文件`<stddef.h>`中定义。）其中，对象可以是变量、数组或结构；类型可以是基本类型，也可以是派生类型。

条件编译语句`#if`中不能使用`sizeof`，因为预处理器不对类型名进行分析。但预处理器并不计算`#define`语句中的表达式，因此，在`#define`中使用`sizeof`是合法的。

## 4. 指向结构的指针

但是，千万不要认为结构的长度等于各成员长度的和。因为不同的对象有不同的对齐要求，所以，结构中可能会出现未命名的“空穴”（hole）。使用`sizeof`运算符可以返回正确的对象长度。

...具体采用哪种写法属于个人的习惯问题，可以选择自己喜欢的方式并始终保持自己的风格。

## 5. 自引用结构

一个包含其自身实例的结构是非法的。

...自引用结构的一种变体：两个结构相互引用。

对齐要求一般比较容易满足，只需要确保分配程序始终返回满足所有对齐限制要求的指针就可以了，其代价是牺牲一些存储空间。

## 6. 表查找

## 7. 类型定义（typedef）

C语言提供了一个称为`typedef`的功能，它用来建立新的数据类型名。

注意，`typedef`中声明的类型在变量名的位置出现，而不是紧接在关键字`typedef`之后。`typedef`在语法上类似于存储类`extern`、`static`等。

从任何意义上讲，`typedef`声明并没有创建一个新类型，它只是为某个已存在的类型增加了一个新的名称而已。`typedef`声明也没有增加任何新的语义：通过这种方式声明的变量与通过普通方式声明的变量具有相同的属性。实际上，`typedef`类似于`#define`语句，但由于`typedef`是由编译器解释的，因此它的文本替换功能要超过预处理器的能力。

除了表达方式更简洁之外，使用`typedef`还有另外两个重要原因。首先，它可以使程序参数化，以提高程序的可移植性。如果`typedef`声明的数据类型同机器有关，那么，当程序移植到其他机器上时，只需改变`typedef`类型定义就可以了。一个经常用到的情况是，对于各种不同大小的整数值来说，都使用通过`typedef`定义的类型名，然后，分别为各个不同的宿主机选择一组合适的`short`、`int`和`long`类型大小即可。标准库中有一些例子，例如`size_t`和`ptrdiff_t`等。

`typedef`的第二个作用是为程序提供更好的说明性。