---
title: arch linux下的bochs安装及bootloader测试 
date: 2022-02-01 01:38:07 +0800
tags: bochs bootloader
categories: [bochs, bootloader]
---

`bochs`，`qemu`等都可以做模拟机供我们开发操作系统，在使用时，我们需要调试程序，所以采用源码编译，目前最新的版本为2.7，可以从[此](https://udomain.dl.sourceforge.net/project/bochs/bochs/2.7/bochs-2.7.tar.gz)下载。

# 安装

首先安装依赖(arch linux不需要安装依赖，直接编译就好)：

```console
sudo apt-get install build-essential xorg-dev bison libgtk2.0-dev g++
```

其次将源码解压缩，打开调试支持

```console
tar -zxvf bochs-2.7.tar.gz
cd bochs-2.7
sudo ./configure --enable-debugger
```

清除配置时生成的中间文件，并重新编译、安装

```console
# 如果不进行清除，则会编译失败，不是很清楚造成这个问题的原因；
make clean
make
sudo make install
```

# 测试

我们自己写一个最简单的`nasm`汇编程序，制作为`bootloader`，使用`bochs`加载以达到测试目的。

程序源码如下，原理为直接向显存（`0xb800`）写字符串，其中每字符占两个字节，显存写完后使用`jmp`指令将程序卡住；另外制作`bootloader`需要写满一个扇区，所以使用`0x00`填充，并以`0x55aa`结束。

```nasm?linenums
mov ax, 0xb800
mov ds, ax

mov byte [0x00], 'H'
mov byte [0x02], 'e'
mov byte [0x04], 'l'
mov byte [0x06], 'l'
mov byte [0x08], 'o'
mov byte [0x0a], ','
mov byte [0x0c], 'O'
mov byte [0x0e], 'S'
mov byte [0x10], '!'

jmp $

times 510-($-$$) db 0
                 db 0x55,0xaa
```

程序的编译、镜像的制作与`bochs`的执行都需要人工操作，可写成`makefile`以简化操作；在执行`make build`制作镜像之前，需要使用`bximage`(随`bochs`一起安装的)制造一个默认镜像，我们制造一个名为`test.img`，大小`1.44MB`的软盘：

```makefile?linenums
test.bin: test.asm
        nasm -o test.bin test.asm

.PHONEY: build
build:
        dd if=test.bin of=test.img bs=512 count=1 conv=notrunc

.PHONEY: run
run:
        bochs -f bochsrc
```

我们可以使用`make`来编译源码，使用`make build`将`test.bin`写入`test.img`，使用`make run`来启动`bochs`。

我们为`bochs`手动指明启动参数，在当前目录写如下配置文件：

```bash?linenums
###############################################################
# Configuration file for Bochs
###############################################################

# how much memory the emulated machine will have
megs: 32

# filename of ROM images
romimage: file=/usr/local/share/bochs/BIOS-bochs-latest
vgaromimage: file=/usr/local/share/bochs/VGABIOS-lgpl-latest

# what disk images will be used
floppya: 1_44=test.img, status=inserted

# choose the boot disk.
boot: floppy

# where do we send log messages?
# log: bochsout.txt

# disable the mouse
mouse: enabled=1

# enable key mapping, using US layout as default.
keyboard:keymap=/usr/local/share/bochs/keymaps/x11-pc-us.map
```

