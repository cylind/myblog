---
title: gcc简易用法小结
date: 2021-03-13 09:34:06
tags: 计算机系统
---

GCC, the GNU Compiler Collection，顾名思义，这是一个编译器集合。它支持多种语言的编译，比如 [C](https://gcc.gnu.org/c99status.html), [C++](https://gcc.gnu.org/projects/cxx-status.html), Objective-C, [Fortran](https://gcc.gnu.org/fortran/), Go等。

基于我学习计算机系统的目的，那么下面对gcc的简易用法，主要就是围绕C和汇编语言展开。初学gcc和计算机系统，有不完善不正确的地方，以后再慢慢完善改进。

<!-- more -->

## 最简单的用法

以编译一个最简单的hello world的c程序 `hello.c`为例

```c
#include <stdio.h>
int main(void)
{
    printf("Hello World!\n");
    return 0;
}
```

用gcc编译它的最简单命令就是：

```bash
gcc hello.c
```

执行这个命令后，就会生成一个编译好的可执行的二进制文件 `a.out` ，运行 `a.out` 就会输出Hello World!

## 分步解析

其实，这个简单的  `gcc hello.c`  命令的背后，包括了多个处理步骤：预处理、编译、汇编和链接。我们可以添加更多参数，来看看gcc编译过程中的上述多个处理步骤中每一步的具体作用和效果。

### 预处理

```
gcc -E hello.c -o hello.i
```

>       -E  Stop after the preprocessing stage; do not run the compiler proper.  The output is in
>                the form of preprocessed source code, which is sent to the standard output.

>-o file
>     Place output in file file.  This applies to whatever sort of output is being produced,
>     whether it be an executable file, an object file, an assembler file or preprocessed C
>     code.

>       file.c
>           C source code that must be preprocessed.
>    
>       file.i
>           C source code that should not be preprocessed.

从上面在gcc手册中摘抄的解释，可以看出，`-E` 参数表示仅对源码进行预处理，即仅进行源代码检测，并替换我们源程序中的宏定义；`-o` 参数表示输出文件的存放路径，它是个全局参数；最后输出的 `.i` 文件表示一种不需进行预处理的代码文本文件，从这里我们也可以看出，gcc一定程度上依赖文件后缀名来判断文件类型。

### 编译

```
gcc -S hello.i -o hello.s
```

>-S  Stop after the stage of compilation proper; do not assemble.  The output is in the
>     form of an assembler code file for each non-assembler input file specified.
>     By default, the assembler file name for a source file is made by replacing the suffix
>     .c, .i, etc., with .s.

>       file.s
>                Assembler code.

`-S` 参数表示只编译（这里的编译指的是将高级语言转换成汇编语言的过程），而不进行后续步骤。执行完毕后，产生`.s` 文件，这个文本文件里的内容是汇编代码。

### 汇编

```
gcc -c hello.s -o hello.o
```

>       -c  Compile or assemble the source files, but do not link.  The linking stage simply is
>                not done.  The ultimate output is in the form of an object file for each source file.

>           By default, the object file name for a source file is made by replacing the suffix .c,
>           .i, .s, etc., with .o.

`-c` 参数表示只用汇编器进行汇编，而不进行后续步骤。汇编的过程，就是将汇编指令替换成机器指令（机器码）。之所以说是替换，是因为汇编指令和机器指令一一对应，可以说汇编指令就是机器指令的符号化映射。

执行完毕后，会得到一个可重定位的二进制文件（包含了相对地址，该文件分配到了相对内存地址，从来地址0开始，并非机器运行时的真实内存地址或虚拟地址，但知道了相对位置，后面在对应插入内容是可行的，所以说是可重定位），里面包含一对的机器码和数据。

> - 存放程序的为代码段，存放数据的为数据段
> -    真实的内存单元地址称为物理地址/绝对内存地址，而程序中的地址为逻辑地址
> 由于程序并不知道自己会被加载到哪，因此访存如果用绝对地址将会出错，在执行程序时就需要**程序重定位**这个操作。

现在得到的虽然是包含了机器码的二进制文件，但是还不能直接运行，因为还没有程序中包含的printf对应的二进制文件包含进来，我们预处理的时候只是在源代码中插入了`stdio` 的头文件而已。而且还尚未分配可用的内存地址给这个程序，也就是说，要能被正常运行的话，当它被加载到内存中去的时候，cpu要知道它应该被加载到内存的那个地方，它所包含的每条指令或数据在内存中的确切地址。

### 链接

```
gcc hello.o -o hello
```

最后一步，链接，就是给之前的`.o`二进制文件链接上要用到的其他的 `.o` ，比如`printf.o` ，并给程序分配确切的内存地址（虚拟内存地址），最后生成的才是可执行的二进制文件。

## 分部运行

gcc它是一个编译器集合套件，编译一个程序的时候包含预处理、编译、汇编和链接这四个步骤，其实每一步骤，它都有不同的组件来执行的，分别对于预处理器、编译器、汇编器、链接器。使用高级语言的时候，我们可以使用gcc一步到位，但当我们用汇编语言的时候，大可不必用gcc再加一堆参数，而是直接单独地使用它的组件，比如汇编器（as）、链接器（ld）等，来完成我们的工作。

汇编器，as，对应gcc汇编那一步

```bash
as asm_code.s -o asm_code.o
```

链接器，ld，对应gcc链接那一步

```bash
ld asm_code.o -o asm_code
```

其中，`-o`参数和gcc中的是一样的。

## 其他常用参数

- `-x language`：强制编译器指定的语言编译器来编译某个源程序。
- `-g` 指示编译器，在编译的时候，产生调试信息，以便后面用gdb对其进行调试。
- `-std=` 指定编译器采取的标准，比如设置`-std=c99` 让编译器以c99标准来编译c代码。

## 参考

[gcc常用选项详解](https://markrepo.github.io/tools/2018/06/25/gcc/)

[gcc编译器学习记录](https://github.com/guodongxiaren/LinuxTool/blob/master/gcc.md)

[Linux GCC常用命令](https://www.cnblogs.com/ggjucheng/archive/2011/12/14/2287738.html)

https://gcc.gnu.org/

[关于 C 语言编译流程 PCAL 的总结](https://junhaow.com/2018/06/10/028%20%7C%20%E5%85%B3%E4%BA%8E%20C%20%E8%AF%AD%E8%A8%80%E7%BC%96%E8%AF%91%E6%B5%81%E7%A8%8B%20PCAL%20%E7%9A%84%E6%80%BB%E7%BB%93/)

[x86架构与汇编语言](https://chhzh123.github.io/blogs/2019-03-07-x86-asm/)

[C语言的编译与链接 - gcc,ld,ar等工具的介绍](https://whatsrtos.github.io/blog_archive/[C]%20C%E8%AF%AD%E8%A8%80%E7%9A%84%E7%BC%96%E8%AF%91%E4%B8%8E%E9%93%BE%E6%8E%A5/)