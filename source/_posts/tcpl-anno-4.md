---
title: The C Programming Language - annotation, 4
tags:
  - c
  - language
  - programming
categories:
  - - c
    - language
  - - language
    - c
date: 2024-09-03 22:19:58
---

## Function and Program Structure

函数可以把大的计算任务分解成若干个较小的任务，程序设计人员可以基于函数进一步构造程序，而不需要重新编写一些代码。一个设计得当的函数可以把程序中不需要了解的具体操作细节隐藏起来，从而使整个程序结构更加清晰，并降低修改程序的难度。

C语言在设计中考虑了函数的高效性与易用性这两个因素。C语言程序一般都由许多小的函数组成，而不是由少量较大的函数组成。一个程序可以保存在一个或者多个源文件中。各个文件可以单独编译，并可以与库中已编译过的函数一起加载。

ANSI标准对C语言所做的最明显的修改是函数声明与函数定义这两方面。目前C语言已经允许在声明函数时声明参数的类型。为了使函数的声明与定义相适应，ANSI标准对函数定义的语法也做了修改。基于该原因，编译器就有可能检测出比以前的C语言版本更多的错误。并且，如果参数声明得当，程序可以自动地进行适当的强制类型转换。

ANSI标准进一步明确了名字的作用域规则，特别要求每个外部对象只能有一个定义。初始化的适用范围也更加广泛了，自动数组与结构都可以进行初始化。

C语言预处理器的功能也得到了增强。新的预处理器包含一组更完整的条件编译指令（一种通过宏参数创建带引号的字符串的方法），对宏扩展过程的控制更严格。

## 1. 函数的基本知识

尽管我们可以把所有的代码都放在主程序`main`中，但更好的做法是，利用其结构把每一部分设计成一个独立的函数。分别处理3个小的部分比处理一个大的整体更容易，因为这样可以把不相关的细节隐藏在函数中，从而减少了不必要的相互影响的机会，并且，这些函数也可以在其他程序中使用。

函数的定义形式如下：

```c 函数的定义
返回值类型 函数名(参数声明表)
{
    声明和语句
}
```

函数定义中的各构成部分都可以省略。最简单的函数如下所示：

```
dummy() {}
```

该函数不执行任何操作也不返回任何值。这种不执行任何操作的函数有时很有用，它可以在程序开发期间用以保留位置（留待以后填充代码）。如果函数定义中省略了返回值类型，则默认为`int`类型。

程序可以看成是变量定义和函数定义的集合。函数之间的通信可以通过参数、函数返回值以及外部变量进行。函数在源文件中出现的次序可以是任意的。只要保证每一个函数不被分离到多个文件中，源程序就可以分成多个文件。

被调用函数通过`return`语句向调用者返回值，`return`语句的后面可以跟任何表达式：

```
return expression;
```

在必要时，**表达式**将被转换为函数的返回值类型。**表达式**两边通常加一对圆括号，此处的括号是可选的。

调用函数可以忽略返回值。并且，`return`语句的后面也不一定需要表达式。当`return`语句的后面没有表达式时，函数将不向调用者返回值。当被调用函数执行到最后的右花括号而结束执行时，控制同样也会返回给调用者（不返回值）。如果某个函数从一个地方返回时有返回值，而从另一个地方返回时没有返回值，该函数并不非法，但可能是一种出问题的征兆。在任何情况下，如果函数没有成功地返回一个值，则它的“值”肯定是无用的。

...主程序`main`返回了一个状态。该返回值可以在该程序的环境中使用。

在不同的系统中，保存在多个源文件中的C语言程序的编译与加载机制是不同的。

## 2. 返回非整型值的函数

首先，由于函数的返回值类型不是`int`，因此函数必须声明返回值的类型。返回值的类型名应放在函数名字之前。

其次，调用函数必须知道函数返回的是非整型值，这一点也是很重要的。为了达到该目的，一种方法是在调用函数中显式声明函数。

函数的声明与定义必须一致。如果函数与调用它的主函数`main`放在同一源文件中，并且类型不一致，编译器就会检测到该错误。但是，如果函数是单独编译的（这种可能性更大），这种不匹配的错误就无法检测出来，函数将返回`double`类型的值，而`main`函数却将返回值按照`int`类型处理，最后的结果值毫无意义。

根据前面有关函数的声明如何与定义保持一致的讨论，发生不匹配现象似乎很令人吃惊。其中的一个原因是，如果没有函数原型，则函数将在第一次出现的表达式中被隐式声明。如果先前没有声明过的一个名字出现在某个表达式中，并且其后紧跟一个左圆括号，那么上下文就会认为该名字是一个函数名字，该函数的返回值将被假定为`int`类型，但上下文并不对其参数作任何假设。并且，如果函数声明中不包含参数，那么编译程序也不会对函数的参数作任何假设，并会关闭所有的参数检查。对空参数表的这种特殊处理是为了使新的编译器能编译比较老的C语言程序。不过，在新编写的程序中这么做是不提倡的。如果函数带有参数，则要声明它们；如果没有参数，则使用`void`进行声明。

在下列形式的`return`语句中：

```c
return (expression);
```

其中，表达式的值在返回之前将被转换为函数的类型。这种操作可能会丢失信息，某些编译器可能会对此给出警告信息。在该函数中，由于采用了类型转换的方法显式表明了所要执行的转换操作，因此可以防止有关的警告信息。

## 3. 外部变量

C语言程序可以看成由一系列的外部对象构成，这些外部对象可能是变量或函数。形容词 external 与 internal 是相对的，internal 用于描述定义在函数内部的函数参数及变量。外部变量定义在函数之外，因此可以在许多函数中使用。由于C语言不允许在一个函数中定义其他函数，因此函数本身是“外部的”。默认情况下，外部变量与函数具有下列性质：通过同一个名字对外部变量的所有引用（即使这种引用来自于单独编译的不同函数）实际上都是引用同一个对象（标准中把这一性质称为**外部连接**）。

因为外部变量可以在全局范围内访问，这就为函数之间的数据交换提供了一种可以代替函数参数与返回值的方式。任何函数都可以通过名字访问一个外部变量，当然这个名字需要通过某种方式进行声明。

如果函数之间需要共享大量的变量，使用外部变量要比使用一个很长的参数表更方便、有效。但是，这样做必须非常谨慎，因为这种方式可能对程序结构产生不良的影响，而且可能会导致程序中各个函数之间具有太多的数据联系。

外部变量的用途还表现在它们与内部变量相比具有更大的作用域和更长的生存期。自动变量只能在函数内部使用，从其所在的函数被调用时变量开始存在，在函数退出时变量也将消失。而外部变量是永久存在的，它们的值在一次函数调用到下一次函数调用之间保持不变。因此，如果两个函数必须共享某些数据，而这两个函数互不调用对方，这种情况下最方便的方式便是把这些共享数据定义为外部变量，而不是作为函数参数传递。

## 4. 作用域规则

构成C语言程序的函数与外部变量可以分开进行编译。一个程序可以存放在几个文件中，原先已编译过的函数可以从库中进行加载。

名字的**作用域**指的是程序中可以使用该名字的部分。对于在函数开头声明的自动变量来说，其作用域是声明该变量名的函数。不同函数中声明的具有相同名字的各个局部变量之间没有任何关系。函数的参数也是这样的，实际上可以将它看作是局部变量。

外部变量或函数的作用域从声明它的地方开始，到其所在的（待编译的）文件的末尾结束。

另一方面，如果要在外部变量的定义之前使用该变量，或者外部变量的定义与变量的使用不在同一个源文件中，则必须在相应的变量声明中强制性地使用关键字`extern`。

将外部变量的**声明**与**定义**严格区分开来很重要。变量声明用于说明变量的属性（主要是变量的类型），而变量定义除此以外还将引起存储器的分配。

在一个源程序的所有源文件中，一个外部变量只能在某个文件中定义一次，而其他文件可以通过`extern`声明来访问它（定义外部变量的源文件中也可以包含对该外部变量的`extern`声明）。外部变量的定义中必须指定数组的长度，但`extern`声明则不一定要指定数组的长度。

外部变量的初始化只能出现在其定义中。

## 5. 头文件

...把程序分割到若干个源文件中的情况，如果该程序的各组成部分很长，那么这么做还是有必要的。...主要是考虑在实际的程序中，它们分别来自于单独编译的库。

此外，还必须考虑定义和声明在这些文件之间的共享问题。我们尽可能把共享的部分集中在一起，这样就只需要一个副本，改进程序时也容易保证程序的正确性。我们把这些公共部分放在头文件中，在需要使用该头文件时通过`#include`指令将它包含进来。

我们对下面两个因素进行了折衷：一方面是我们期望每个文件只能访问它完成任务所需的信息；另一方面是现实中维护较多的头文件比较困难。我们可以得出这样一个结论：对于某些中等规模的程序，最好只用一个头文件存放程序中各部分共享的对象。较大的程序需要使用更多的头文件，我们需要精心地组织它们。
