---
title: The C Programming Language - annotation, 3
tags:
  - c
  - language
  - programming
categories:
  - - c
    - language
  - - language
    - c
date: 2024-07-03 20:29:56
---

## Control Flow

程序语言中的控制流语句用于控制各计算操作执行的次序。

## 3.1 语句与程序块

表达式之后加上一个分号（`;`），它们就变成了**语句**。

在C语言中，分号是语句结束符。

用一对花括号“`{`”与“`}`”把一组声明和语句括在一起就构成了一个**复合语句**（也叫作**程序块**），复合语句在语法上等价于单条语句。函数体中被花括号括起来的语句便是明显一例。`if`、`else`、`while`与`for`之后被花括号括住的多条语句也是类似的例子。右花括号用于结束程序块，其后不需要分号。

## 3.2 `if-else`语句

`if-else`语句用于条件判定。

```c if-else语法
if (expression)
    statement1
else
    statement2
```

其中`else`部分是可选的。该语句执行时，先计算**表达式**的值，如果其值为真（即表达式的值为非0），则执行**语句1**；如果其值为假（即表达式的值为0），并且该语句包含`else`部分，则执行**语句2**。

由于`if`语句只是简单测试表达式的数值，因此可以对某些代码的编写进行简化。最明显的例子是用如下写法`if (expression)`来代替`if (expression != 0)`。某些情况下这种形式是自然清晰的，但也有某些情况下可能会含义不清。

因为`if-else`语句的`else`部分是可选的，所以在嵌套的`if`语句中省略它的`else`部分将导致歧义。解决的方法是将每个`else`与最近的前一个没有`else`配对的`if`进行匹配。...如果这不符合我们的意图，则必须使用花括号强制实现正确的匹配关系。

...建议在有`if`语句嵌套的情况下使用花括号。

...从语法上讲，跟在`if`后面的应该是一条语句。

## 3.3 `else-if`语句

```c else-if语法
if (expression)
    statement
else if (expression)
    statement
else if (expression)
    statement
else if (expression)
    statement
else
    statement
```

这种`if`语句序列是编写多路判定最常用的方法。其中的各**表达式**将被依次求值，一旦某个表达式结果为真，则执行与之相关的**语句**，并终止整个语句序列的执行。同样，其中各语句既可以是单条语句，也可以是用花括号括住的复合语句。

最后一个`else`部分用于处理“上述条件均不成立”的情况或默认情况，也就是当上面各条件都不满足时的情形。有时候并不需要针对默认情况执行显式的操作，这种情况下，可以把该结构末尾的`else`部分省略掉；该部分也可以用来检查错误，以捕获“不可能”的条件。

## 3.4 `switch`语句

`switch`语句是一种多路判定语句，它测试表达式是否与一些**常量**整数值中的某一个值匹配，并执行相应的分支动作。

```c switch语法
switch (expression) {
    case const expression: statement sequences
    case const expression: statement sequences
    default: statement sequences
}
```

每一个分支都由一个或多个整数值常量或常量表达式标记。如果某个分支与表达式的值匹配，则从该分支开始执行。各分支表达式必须互不相同。如果没有哪一分支能匹配表达式，则执行标记为`default`的分支。`default`分支是可选的。如果没有`default`分支也没有其他分支与表达式的值匹配，则该`switch`语句不执行任何动作。各分支及`default`分支的排列次序是任意的。

`break`语句将导致程序的执行立即从`switch`语句中退出。在`switch`语句中，`case`的作用只是一个标号，因此，某个分支中的代码执行完后，程序将进入下一分支继续执行，除非在程序中显式地跳转。跳出`switch`语句最常用的方法是使用`break`语句与`return`语句。

依次执行各分支的做法有优点也有缺点。好的一面是它可以把若干个分支组合在一起完成一个任务。但是，正常情况下为了防止直接进入下一个分支执行，每个分支后必须以一个`break`语句结束。从一个分支直接进入下一个分支执行的做法并不健全，这样做在程序修改时很容易出错。除了一个计算需要多个标号的情况下，应尽量减少从一个分支直接进入下一个分支执行这种用法，在不得不使用的情况下应该加上适当的程序注释。

作为一种良好的程序设计风格，在`switch`语句最后一个分支（即`default`分支）的后面也加上一个`break`语句。这样做在逻辑上没有必要，但当我们需要向该`switch`语句后添加其他分支时，这种防范措施会降低犯错误的可能性。