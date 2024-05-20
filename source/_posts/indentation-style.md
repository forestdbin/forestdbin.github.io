---
title: 缩进风格
tags:
  - style
  - programming
date: 2024-05-20 22:26:55
categories:
---

## Indentation style

本文只记录“是什么”，不讨论“好”“坏”。

In computer programming, [indentation style](https://en.wikipedia.org/wiki/Indentation_style) is a __convention__, a.k.a. __style__, governing the indentation of blocks of source code that is generally intended to _convey_ structure.

Generally, an indentation style uses the same width of whitespace (indentation size) before each line of a group of code so that they _appear_ to be related.

As whitespace consists of both `space` and `tab` characters, a programmer can _choose_ which to use - often entering them via the _space key_ or _tab key_ on the keyboard.

### K&R

The position of braces is less important, although people hold passionate beliefs. We have chosen one of several popular styles. Pick a style that suits you, then use it consistently.

```c K&R
while (x == y) {
    foo();
    bar();
}
```

```c
int main(int argc, char *argv[])
{
    while (x == y) {
        do_something();
        do_something_else();
        if (some_error)
            fix_issue(); // single-statement block without braces
        else
            continue_as_usual();
    }
    final_thing();
}
```

### One True Brace / 1TBS / OTBS

```c One True Brace
bool is_negative(x) {
    if (x < 0) {
        return true;
    } else {
        return false;
    }
}
```

### Linux kernel

```c Linux kernel
int power(int x, int y)
{
	int result;
	if (y < 0) {
		result = 0;
	} else {
		result = 1;
		while (y-- > 0)
			result *= x;
	}
	return result;
}
```

### Stroustrup

```c++ Stroustrup
if (x < 0) {
    puts("Negative");
    negative(x);
}
else {
    puts("Non-negative");
    nonnegative(x);
}
```

```c++ Stroustrup
class Vector {
public:
    // construct a Vector
    Vector(int s) :elem(new double[s]), sz(s) { }
    // element access: subscripting
    double& operator[](int i) { return elem[i]; }
private:
    // pointer to the elements
    double * elem;
    // number of elements
    int sz;
};
```

### Allman

```c Allman
while (x == y)
{
    foo();
    bar();
}
```

### GNU

```c GNU
while (x == y)
  {
    foo();
    bar();
  }
```

### Whitesmiths

```c Whitesmiths
while (x == y)
    {
    foo();
    bar();
    }
```

### Ratliff

```c Ratliff
while (x == y) {
    foo();
    bar();
    }
```

### Horstmann

```c Horstmann
while (x == y)
{   foo();
    bar();
}
```

### Pico

```c Pico
while (x == y)
{   foo();
    bar(); }
```

### Lisp

```c Lisp
while (x == y)
  { foo();
    bar(); }
```

### APL

```c APL
#define W(c,b) {while(c){b}}
W(x==y,s();se();)
```

### Python

```python Python
while x == y:
    foo()
    bar()
```
