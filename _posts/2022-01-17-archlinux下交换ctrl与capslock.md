---
title: arch linux下交换control与capslock 
date: 2022-01-17 00:05:04 +0800
tags: archlinux 键盘策略
categories: [archlinux, 键盘策略]
---

通用的`linux`系统做法需要用到`xmodmap`工具，具体操作如下：

编辑一个`Xmodmap`文件用于存储`xmodmap`脚本

```console
vim ~/.Xmodmap
```

脚本内容如下：

```bash?linenums
remove Lock = Caps_Lock
remove Control = Control_L
keysym Control_L = Caps_Lock
keysym Caps_Lock = Control_L
add Lock = Caps_Lock
add Control = Control_L
```

如果只是想单次验证，运行如下命令即可
```console
xmodmap ~/.Xmodmap
```

添加至`bashrc`里，则在开启终端时自动生效

```bash?linenums
if [ -f ~/.Xmodmap ]; then xmodmap ~/.Xmodmap; fi
```

由于我们使用`i3-wm`，所以直接在`~/.config/i3/config`中配置即可:

```bash?linenums
exec_always ~/.Xmodmap
```
