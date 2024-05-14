---
title: hello.c
date: 2024-05-14 21:08:52
tags:
  - c
  - hello
categories: hello world
---

```c hello.c
#include <stdio.h>

main()
{
    printf("hello, world\n");
}
```

```bash
$ cc hello.c
hello.c:3:1: warning: return type defaults to ‘int’ [-Wimplicit-int]
    3 | main()
      | ^~~~
$ ./a.out
hello, world
$
```
