---
title: arch linux下的纯汇编使用 
date: 2022-02-01 03:53:10 +0800
tags: archlinux nasm 
categories: [archlinux, nasm]
---

首先安装汇编器，我们使用`nasm`，`arch linux`使用如下命令即可安装

```console
sudo pacman -S nasm
```

创建一个名为`helloworld.asm`的汇编源文件用于存储源代码，使用汇编器及链接器生成目标文件及可重定位文件

```console
vim helloworld.asm
nasm -f elf -o helloworld.o helloworld.asm
ld -m elf_i386 -s -o helloworld helloworld.o
./helloworld
```

测试代码如下，该例程用于输出一个字符串至标准输出设备上

```nasm?linenums
segment .data
msg  db   "Hello,world!",10
len  equ  $-msg
 
segment .text
global _start
 
_start:
   mov  eax,4
   mov  ebx,1
   mov  ecx,msg
   mov  edx,len
   int  80h
 
   mov  eax,1
   mov  ebx,0
   int  80h
```
