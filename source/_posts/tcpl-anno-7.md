---
title: The C Programming Language - annotation, 7
tags:
  - c
  - language
  - programming
categories:
  - - c
    - language
  - - language
    - c
date: 2024-11-20 22:01:15
---

## Input and Output

输入/输出功能并不是C语言本身的组成部分。...标准库...输入/输出函数、字符串处理函数、存储管理函数与数学函数。

ANSI标准精确地定义了这些库函数，所以，在任何可以使用C语言的系统中都有这些函数的兼容形式。如果程序的系统交互部分仅仅使用了标准库提供的功能，则可以不经修改地从一个系统移植到另一个系统中。

## 1. 标准输入/输出

标准库实现了简单的文本输入/输出模式。文本流由一系列行组成，每一行的结尾是一个换行符。如果系统没有遵循这种模式，则标准库将通过一些措施使得该系统适应这种模式。例如，标准库可以在输入端将回车符和换页符都转化为换行符，而在输出端进行反向转换。

最简单的输入机制是使用`getchar`函数从**标准输入**中（一般为键盘）一次读取一个字符：

```c
int getchar(void)
```

`getchar`函数在每次被调用时返回下一个输入字符。若遇到文件结尾，则返回`EOF`。符号常量`EOF`在头文件`<stdio.h>`中定义，其值一般为-1，但程序中应该使用`EOF`来测试文件是否结束，这样才能保证程序同`EOF`的特定值无关。

在许多环境中，可以使用符号`<`来实现输入重定向，它将把键盘输入替换为文件输入：如果程序prog中使用了函数`getchar`，则命令行`prog <infile`将使得程序prog从输入文件infile（而不是从键盘）中读取字符。实际上，程序prog本身并不在意输入方式的改变，并且，字符串“<infile”也并不包含在`argv`的命令行参数中。如果输入通过管道机制来自于另一个程序，那么这种输入切换也是不可见的。比如，在某些系统中，下列命令行：`other prog | prog`将运行两个程序otherprog和prog，并将程序otherprog的标准输出通过管道重定向到程序prog的标准输入上。

函数

```c
int putchar(int)
```

用于输出数据。`putchar(c)`将字符c送至**标准输出**上，在默认情况下，标准输出为屏幕显示。如果没有发生错误，则函数`putchar`将返回输出的字符；如果发生了错误，则返回`EOF`。同样，通常情况下，也可以使用“>outfile”的格式将输出重定向到某个文件中。例如，如果程序prog调用了函数`putchar`，那么命令行`prog >outfile`将把程序prog的输出从标准输出设备重定向到文件中。如果系统支持管道，那么命令行`prog | anotherprog`将把程序prog的输出从标准输出通过管道重定向到程序anotherprog的标准输入中。

函数`printf`也向标准输出设备上输出数据。我们在程序中可以交叉调用函数`putchar`和`printf`，输出将按照函数调用的先后顺序依次产生。

使用输入/输出库函数的每个源程序文件必须在引用这些函数之前包含下列语句：`#include <stdio.h>`。当文件名用一对尖括号`<`和`>`括起来时，预处理将在由具体实现定义的有关位置中查找指定的文件（例如，在UNIX系统中，文件一般放在目录`/usr/include`中）。

许多程序只从一个输入流中读取数据，并且只向一个输出流中输出数据。对于这样的程序，只需要使用函数`getchar`、`putchar`和`printf`实现输入/输出即可，并且对程序来说已经足够了。特别是，如果通过重定向将一个程序的输出连接到另一个程序的输入，仅仅使用这些函数就足够了。

## 2. 格式化输出——`printf`函数

输出函数`printf`将内部数值转换为字符的形式。

```
int printf(char *format, arg1, arg2, ...)
```

函数`printf`在输出格式`format`的控制下，将其参数进行转换与格式化，并在标准输出设备上打印出来。它的返回值为打印的字符数。

格式字符串包含两种类型的对象：普通字符和转换说明。在输出时，普通字符将原样不动地复制到输出流中，而转换说明并不直接输出到输出流中，而是用于控制`printf`参数的转换和打印。每个转换说明都由一个百分号字符（即`%`）开始，并以一个转换字符结束。在字符`%`和转换字符中间可能依次包含下列组成部分：

- 负号，用于指定被转换的参数按照左对齐的形式输出。
- 数，用于指定最小字段宽度。转换后的参数将打印不小于最小字段宽度的字段。如果有必要，字段左边（如果使用左对齐的方式，则为右边）多余的字段位置用空格填充以保证最小字段宽。
- 小数点，用于将字段宽度和精度分开。
- 数，用于指定精度，即指定字符串中要打印的最大字符数、浮点数小数点后的位数、整数最少输出的数字数目。
- 字母`h`或`l`，字母`h`表示将整数作为`short`类型打印，字母`l`表示将整数作为`long`类型打印。

转换字符：

- d, i：`int`类型；十进制数
- o：`int`类型；无符号八进制数（没有前导`0`）
- x, X：`int`类型；无符号十六进制数（没有前导`0x`或`0X`），10~15分别用`abcdef`或`ABCDEF`表示
- u：`int`类型；无符号十进制数
- c：`int`类型；单个字符
- s：`char *`类型；顺序打印字符串中的字符，直到遇到`'\0'`或已打印了由精度指定的字符数为止
- f：`double`类型；十进制小数 [-]_m.dddddd_，其中 _d_ 的个数由精度指定（默认值为6）
- e, E：`double`类型；[-]_m.dddddd e/E+/-xx_，其中 _d_ 的个数由精度指定（默认值为6）
- g, G：`double`类型；如果指数小于-4或大于等于精度，则用`%e/E`格式输出，否则用`%f`格式输出。尾部的0和小数点不打印
- p：`void *`类型；指针（取决于具体实现）
- %：不转换参数；打印一个百分号`%`

如果`%`后面的字符不是一个转换说明，则该行为是未定义的。

在转换说明中，宽度或精度可以用星号`*`表示，这时，宽度或精度的值通过转换下一参数（必须为`int`类型）来计算。

注意：函数`printf`使用第一个参数判断后面参数的个数及类型。如果参数的个数不够或者类型错误，则将得到错误的结果。

函数`sprintf`执行的转换和函数`printf`相同，但它将输出保存到一个字符串中：

```
int sprintf(char *string, char *format, arg1, arg2, ...)
```

`sprintf`函数和`printf`函数一样，按照`format`格式格式化参数序列`arg1`、`arg2`、...，但它将输出结果存放到`string`中，而不是输出到标准输出中。当然，`string`必须足够大以存放输出结果。

## 3. 变长参数表

函数`printf`的正确声明形式为：

```
int printf(char *fmt, ...)
```

其中，省略号表示参数表中参数的数量和类型是可变的。省略号只能在出现在参数表的尾部。

标准头文件`<stdarg.h>`中包含一组宏定义，它们对如何遍历参数表进行了定义。该头文件的实现因不同的机器而不同，但提供的接口是一致的。

`va_list`类型用于声明一个变量，该变量将依次引用各参数。...将该变量称为`ap`，意思是“参数指针”。宏`va_start`将`ap`初始化为指向第一个无名参数的指针。在使用`ap`之前，该宏必须被调用一次。参数表必须至少包含一个有名参数，`va_start`将最后一个有名参数作为起点。

每次调用`va_arg`，该函数都将返回一个参数，并将`ap`指向下一个参数。`va_arg`使用一个类型名来决定返回的对象类型、指针移动的步长。最后，必须在函数返回之前调用`va_end`，以完成一些必要的清理工作。

## 4. 格式化输入——`scanf`函数

输入函数`scanf`对应于输出函数`printf`，它在与后者相反的方向上提供同样的转换功能。具有变长参数表的函数`scanf`的声明形式如下：

```
int scanf(char *format, ...)
```

`scanf`函数从标准输入中读取字符序列，按照`format`中的格式说明对字符序列进行解释，并把结果保存到其余的参数中。其他所有参数都必须是指针，用于指定经格式转换后的相应输入保存的位置。

当`scanf`函数扫描完其格式串，或者碰到某些输入无法与格式控制说明匹配的情况时，该函数将终止，同时，成功匹配并赋值的输入项的个数将作为函数值返回，所以，该函数的返回值可以用来确定已匹配的输入项的个数。如果到达文件的结尾，该函数将返回`EOF`。注意，返回`EOF`与`0`是不同的，`0`表示下一个输入字符与格式串中的第一个格式说明不匹配。下一次调用`scanf`函数将从上一次转换的最后一个字符的下一个字符开始继续搜索。

另外还有一个输入函数`sscanf`，它用于从一个字符串（而不是标准输入）中读取字符序列：

```
int sscanf(char *string, char *format, arg1, arg2, ...)
```

它按照格式参数`format`中规定的格式扫描字符串`string`，并把结果分别保存到`arg`、`arg2`、...这些参数中。这些参数必须是指针。

格式串通常都包含转换说明，用于控制输入的转换。格式串可能包含下列部分：

- 空格或制表符，在处理过程中将被忽略。
- 普通字符（不包含`%`），用于匹配输入流中下一个非空白符字符。
- 转换说明，依次由一个`%`、一个可选的赋值禁止字符`*`、一个可选的数值（指定最大字段宽度）、一个可选的`h`、`l`或`L`字符（指定目标对象的宽度）以及一个转换字符组成。

转换说明控制下一个输入字段的转换。一般来说，转换结果存放在相应的参数指向的变量中。但是，如果转换说明中有赋值禁止字符`*`，则跳过该输入字段，不进行赋值。输入字段定义为一个不包括空白符的字符串，其边界定义为到下一个空白符或达到指定的字符宽度。这表明`scanf`函数将越过行边界读取输入，因为换行符也是空白符。（空白符包括空格符、横向制表符、换行符、回车符、纵向制表符以及换页符）。

转换字符指定对输入字段的解释。对应的参数必须是指针，这也是C语言通过值调用语义所要求的。

转换字符：

- d：十进制整数；`int *`类型
- i：整数；`int *`类型，可以是八进制（以`0`开头）或十六进制（以`0x`或`0X`开头）
- o：八进制整数（可以以`0`开头，也可以不以`0`开头）；`int *`类型
- u：无符号十进制整数：`unsigned int *`类型
- x：十六进制整数（可以`0x`或`0X`开头，也可以不以`0x`或`0X`开头）；`int *`类型
- c：字符；`char *`类型，将接下来的多个输入字符（默认为1个字符）存放到指定位置。该转换规范通常不跳过空白符。如果需要读入下一个非空白符，可以使用`%1s`
- s：字符串（不加引号）；`char *`类型，指向一个足以存放该字符串（还包括尾部的字符`\0`）的字符数组。字符串的末尾将被添加一个结束符`\0`
- e, f, g：浮点数，它可以包括正负号（可选）、小数点（可选）及指数部分（可选）；`float *`类型
- %：字符`%`；不进行任何赋值操作

转换说明`d`、`i`、`o`、`u`及`x`的前面可以加上字符`h`或`l`。前缀`h`表明参数表的相应参数是一个指向`short`类型而非`int`类型的指针，前缀`l`表明参数表的相应参数是一个指向`long`类型的指针。类似地，转换说明`e`、`f`和`g`的前面也可以加上前缀`l`，它表明参数表的相应参数是一个指向`double`类型而非`float`类型的指针。

字符字面值也可以出现在`scanf`的格式串中，它们必须与输入中相同的字符匹配。

`scanf`函数忽略格式串中的空格和制表符。此外，在读取输入值时，它将跳过空白符（空格、制表符、换行符等等）。如果要读取格式不固定的输入，最后每次读入一行，然后再用`sscanf`将合适的格式分离出来读入。

`scanf`函数可以和其他输入函数混合使用。无论调用哪个输入函数，下一个输入函数的调用将从`scanf`没有读取的第一个字符处开始读取数据。

注意，`scanf`和`sscanf`函数的所有参数都必须是指针。

## 5. 文件访问

标准输入和标准输出是操作系统自动提供给程序访问的。

在读写一个文件之前，必须通过库函数`fopen`打开该文件。`fopen`用外部名与操作系统进行某些必要的连接和通信（我们不必关心这些细节），并返回一个随后可以用于文件读写操作的指针。

该指针称为**文件指针**，它指向一个包含文件信息的结构，这些信息包括：缓冲区的位置、缓冲区中当前 字符的位置、文件的读或写状态、是否出错或是否已经到达文件结尾等等。用户不必关心这些细节，因为`<stdio.h>`中已经定义了一个包含这些信息的结构`FILE`。在程序中只需要按照下列方式声明一个文件指针即可：

```
FILE *fp;
FILE *fopen(char *name, char *mode);
```

在本例中，fp是一个指向结构`FILE`的指针，并且，`fopen`函数返回一个指向结构`FILE`的指针。注意，`FILE`像`int`一样是一个类型名，而不是结构标记。它是通过`typedef`定义的。

在程序中，可以像这样调用`fopen`函数：

```
fp = fopen(name, mode);
```

`fopen`的第一个参数是一个字符串，它包含文件名。第二个参数是访问**模式**，也是一个字符串，用于指定文件的使用方式。允许的模式包括：读（“`r`”）、写（“`w`”）及追加（“`a`”）。某些系统还区分文本文件和二进制文件，对后者的访问需要在模式字符串中增加字符“`b`”。

如果打开一个不存在的文件用于写或追加，该文件将被创建（如果可能的话）。当以写方式打开一个已存在的文件时，该文件原来的内容将被覆盖。但是，如果以追加方式打开一个文件，则该文件原来的内容将保留不变。读一个不存在的文件会导致错误，其他一些操作也可能导致错误，比如试图读取一个无读取权限的文件。如果发生错误，`fopen`将返回`NULL`。

文件被打开后，就需要考虑采用哪些方法对文件进行读写。有多种方法可供考虑，其中，`getc`和`putc`函数最为简单。`getc`从文件中返回下一个字符，它需要知道文件指针，以确定对哪个文件执行操作：

```
int getc(FILE *fp)
```

`getc`函数返回fp指向的输入流中的下一个字符。如果到达文件尾或出现错误，该函数将返回`EOF`。

`putc`是一个输出函数，如下所示：

```
int putc(int c, FILE *fp)
```

该函数将字符c写入到fp指向的文件中，并返回写入的字符。如果发生错误，则返回`EOF`。类似于`getchar`和`putchar`，`getc`和`putc`是宏而不是函数。

启动一个C语言程序时，操作系统环境负责打开3个文件，并将这3个文件的指针提供给该程序。这3个文件分别是标准输入、标准输出和标准错误，相应的文件指针分别为`stdin`、`stdout`和`stderr`，它们在`<stdio.h>`中声明。在大多数环境中，`stdin`指向键盘，而`stdout`和`stderr`指向显示器。`stdin`和`stdout`可以被重定向到文件或管道。

`getchar`和`putchar`函数可以通过`getc`、`putc`、`stdin`及`stdout`定义如下：

```
#define getchar()  getc(stdin)
#define putchar(c) putc((c), stdout)
```

对于文件的格式化输入或输出，可以使用函数`fscanf`和`fprintf`。它们与`scanf`和`printf`函数的区别仅仅在于它们的第一个参数是一个指向所要读写的文件的指针，第二个参数是格式串。如下所示：

```
int fscanf(FILE *fp, char *format, ...)
int fprintf(FILE *fp, char *format, ...)
```

文件指针`stdin`与`stdout`都是`FILE*`类型的对象。但它们是常量，而非变量，因此不能对它们赋值。

函数`int fclose(FILE *fp)`执行和`fopen`相反的操作，它断开由`fopen`函数建立的文件指针和外部名之间的连接，并释放文件指针以供其他文件使用。因为大多数操作系统都限制了一个程序可以同时打开的文件数，所以，当文件指针不再需要时就应该释放，这是一个好的编程习惯。对输出文件执行`fclose`还有另外一个原因：它将把缓冲区中由`putc`函数正在收集的输出写到文件中。当程序正常终止时，程序会自动为每个打开的文件调用`fclose`函数。（如果不需要使用`stdin`与`stdout`，可以把它们关闭掉。也可以通过库函数`freopen`重新指定它们。）

## 6. 错误处理——`stderr`和`exit`

...另一个输出流以与`stdin`和`stdout`相同的方式分派给程序，即`stderr`。即使对标准输出进行了重定向，写到`stderr`中的输出通常也会显示在屏幕上。

...首先，将`fprintf`函数产生的诊断信息输出到`stderr`上，因此诊断信息将会显示在屏幕上，而不是仅仅输出到管道或输出文件中。诊断信息中包含`argv[0]`中的程序名，因此，当该程序和其他程序一起运行时，可以识别错误的来源。

其次，程序使用了标准库函数`exit`，当该函数被调用时，它将终止调用函数的执行。任何调用该程序的进程都可以获取`exit`的参数值，因此，可通过另一个将该程序作为子进程的程序来测试该程序的执行是否成功。按照惯例，返回值0表示一切正常，而非0返回值通常表示出现了异常情况。`exit`为每个已打开的输出文件调用`fclose`函数，以将缓冲区中的所有输出写到相应的文件中。

在主程序`main`中，语句`return expr`等价于`exit(expr)`。但是，使用函数`exit`有一个优点，它可以从其他函数中调用，并且可以查找这些调用。

如果流`fp`中出现错误，则函数`ferror`返回一个非0值。

```
int ferror(FILE *fp)
```

尽管输出错误很少出现，但还是存在的（例如，当磁盘满时），因此，成熟的产品程序应该检查这种类型的错误。

函数`feof(FILE*)`与`ferror`类似。如果指定的文件到达文件结尾，它将返回一个非0值。

```
int feof(FILE *fp)
```

...但对于任何重要的程序来说，都应该让程序返回有意义且有用的值。

## 7. 行输入和行输出

标准库提供了一个输入函数`fgets`，它和`getline`类似。

```
char *fgets(char *line, int maxline, FILE *fp)
```

`fgets`函数从fp指向的文件中读取下一个输入行（包括换行符），并将它存放在字符数组`line`中，它最多可读取`maxline-1`个字符。读取的行将以`\0`结尾保存到数组中。通常情况下，`fgets`返回`line`，但如果遇到了文件结尾或发生了错误，则返回`NULL`。

输出函数`fputs`将一个字符串（不需要包含换行符）写入到一个文件中：

```
int fputs(char *line, FILE *fp)
```

如果发生错误，该函数将返回`EOF`，否则返回一个非负值。

库函数`gets`和`puts`的功能与`fgets`和`fputs`函数类似，但它们是对`stdin`和`stdout`进行操作。有一点我们需要注意，`gets`函数在读取字符串时将删除结尾的换行符（`'\n'`），而`puts`函数在写入字符串时将在结尾添加一个换行符。

ANSI标准规定，`ferror`在发生错误时返回非0值，而`fputs`在发生错误时返回`EOF`，其他情况返回一个非负值。

## 8. 其他函数

### 8.1. 字符串操作函数

...它们都在头文件`<string.h>`中定义。

### 8.2. 字符类别测试和转换函数

头文件`<ctype.h>`中定义了一些用于字符测试和转换的函数。

### 8.3. `ungetc`函数

### 8.4. 命令执行函数

函数`system(char*s)`执行包含在字符串s中的命令，然后继续执行当前程序。s的内容在很大程度上与所用的操作系统有关。`system`函数返回一个整型的状态值，其值来自于执行的命令，并同具体系统有关。在UNIX系统中，返回的状态是`exit`的返回值。

### 8.5. 存储管理函数

函数`malloc`和`calloc`用于动态地分配存储块。函数`malloc`的声明如下：

```
void *malloc(size_t n)
```

当分配成功时，它返回一个指针，该指针指向n字节长度的未初始化的存储空间，否则返回`NULL`。函数`calloc`的声明为

```
void *calloc(size_t n, size_t size)
```

当分配成功时，它返回一个指针，该指针指向的空闲空间足以容纳由n个指定长度的对象组成的数组，否则返回`NULL`。该存储空间被初始化为0。

根据请求的对象类型，`malloc`或`calloc`函数返回的指针满足正确的对齐要求。

`free(p)`函数释放p指向的内存空间，其中，p是此前通过调用`malloc`或`calloc`函数得到的指针。存储空间的释放顺序没有什么限制，但是，如果释放的不是通过调用`malloc`或`calloc`函数得到的指针所指向的存储空间，将是一个很严重的错误。

使用已经释放的存储空间同样是错误的。

### 8.6. 数学函数

头文件`<math.h>`中声明了20多个数学函数。

### 8.7. 随机数发生器函数

函数`rand()`生成介于`0`和`RAND_MAX`之间的伪随机整数序列。其中`RAND_MAX`是在头文件`<stdlib.h>`中定义的符合常量。下面是一种生成大于等于0但小于1的随机浮点数的方法：

```
#define frand() ((double) rand() / (RAND_MAX + 1.0))
```

（如果所用的函数库中已经提供了一个生成浮点随机数的函数，那么它可能比上面这个函数具有更好的统计学特性。）

函数`srand(unsigned)`设置`rand`函数的种子数。
