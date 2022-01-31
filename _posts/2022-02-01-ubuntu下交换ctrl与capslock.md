---
title: ubuntu下交换control与caplock按键 
date: 2022-02-01 01:34:13 +0800
tags: ubuntu 键盘策略
categories: [ubuntu, 键盘策略]
---

Ubuntu下可编辑`/etc/default/keyboard`文件

```console
sudo vim /etc/default/keyboard
```

修改`XKBOPTIONS`一行进行交换

```bash?linenums
XKBOPTIONS="ctrl:swapcpas"
```

重新生效键盘策略

```console
sudo dpkg-reconfigure keyboard-configuration
```
