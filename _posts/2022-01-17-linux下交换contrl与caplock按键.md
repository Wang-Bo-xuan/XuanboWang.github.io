---
title: linux下交换contrl与caplock按键 
date: 2021-01-17 00:05:04 +0800
categories: [linux]
tags: linux, 键盘策略
---

Ubuntu下可编辑/etc/default/keyboard文件

```bash?linenums
sudo vim /etc/default/keyboard
```

```bash?linenums
XKBOPTIONS="ctrl:swapcpas"
```

```bash?linenums
sudo dpkg-reconfigure keyboard-configuration
```

更为通用的`linux`系统的做法如下：

```plaintext?linenums
remove Lock = Caps_Lock
remove Control = Control_L
keysym Control_L = Caps_Lock
keysym Caps_Lock = Control_L
add Lock = Caps_Lock
add Control = Control_L
```

然后使用`xmodmap`指令使其生效即可
