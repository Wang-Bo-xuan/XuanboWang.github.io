---
title: arch linux下的docker环境搭建 
date: 2022-02-01 02:38:17 +0800
tags: archlinux docker
categories: [archlinux, docker]
---

`arch linux`安装`docker`更为简单，运行如下命令即可

```console
# 安装docker
sudo pacman -S docker
# 将当前用户加入docker组
sudo gpasswd -a ${USER} docker
# 提权
sudo chmod 777 /var/run/docker.sock 
# 开机自启
sudo systemctl enable docker.service 
sudo systemctl enable docker.socket
```
