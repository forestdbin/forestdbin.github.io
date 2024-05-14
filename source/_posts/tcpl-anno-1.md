---
title: The C Programming Language - annotation, 1
tags:
  - c
  - language
  - programming
categories:
  - - c
    - language
  - - language
    - c
date: 2024-05-14 21:17:21
---

...引入C语言的基本元素。...一些基本概念，比如变量与常量、算数运算、控制流、函数、基本输入/输出等。

## 1. hello, world

学习一门新程序设计语言的惟一途径就是使用它编写程序。对于所有语言的初学者来说，编写的第一个程序几乎都是相同的，即：

_请打印出下列内容_

> hello, world

尽管这个练习很简单，但对于初学语言的人来说，它任然可能成为一大障碍，因为要实现这个目的，我们首先必须编写程序文本，然后成功地进行编译，并加载、运行，最后输出到某个地方。掌握了这些操作细节以后，其他事情就比较容易了。

```c hello.c
#include <stdio.h>

main()
{
    printf("hello, world\n");
}
```

```bash 如何运行这个程序
$ cc hello.c
$ ./a.out
hello, world
$
```
