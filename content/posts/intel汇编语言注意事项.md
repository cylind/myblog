---
title: intel汇编语言注意事项
date: 2021-02-09 10:58:46
tags: [汇编]
---

学习汇编语言，教材用的王爽的《汇编语言》，也就是intel风格的汇编语言。在此记录汇总一些注意事项。
<!-- more -->

程序模板

```assembly
assume cs:code

code segment
    # 你的代码从这里开始
    mov bx,0020h
    mov ds,bx
    mov bx,0000h
    mov cx,003fh
s:  mov [bx],bx
    inc bx
    loop s
    # 你的代码从这里结束
    mov ax,4c00h
    int 21h
code ends

end
```

1. debug模式下，默认16进制，而程序源代码编写则默认10进制。故在编写汇编程序时，要在数字的后面加h以表示16进制。此外，如果如果该数以字母开头，还要在前面加一个0。
2. 各个寄存器有其特定含义，cx作为循环计数器，循环语句每执行一次，cx自减，直到为零；bx作为基本寄存器，可以用[bx]取偏移地址，而ax不行。

## 参考

[实验4  [bx]和loop的使用](https://www.cnblogs.com/Base-Of-Practice/articles/6883892.html)

