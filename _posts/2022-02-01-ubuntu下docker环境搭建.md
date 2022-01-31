---
title: ubuntu下的docker环境搭建 
date: 2022-02-01 02:07:25 +0800
tags: ubuntu docker
categories: [ubuntu, docker]
---

现在安装`docker`已经非常简单了，可以使用官方安装脚本自动安装：

```console
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

脚本自动安装虽然省事，但是在使用过程中反复出现权限问题，每次使用都需要赋予权限，很是麻烦，貌似手动安装在第一次赋予权限即可，所以推荐使用如下命令进行手动安装：

```console
# 更新apt包索引并安装包以允许apt通过 HTTPS 使用存储库：
sudo apt-get update
sudo apt-get install apt-transport-https \
                     ca-certificates \
                     curl \
                     gnupg-agent \
                     software-properties-common
# 添加Docker官方的GPG密钥：
curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
#使用以下指令设置稳定版仓库
sudo add-apt-repository "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/ $(lsb_release -cs) stable"
#更新 apt 包索引。
sudo apt-get update
#安装最新版本的 Docker Engine-Community 和 containerd，或者转到下一步安装特定版本：
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

安装完成后，因为权限问题，还无法使用`docker`命令，使用下面命令赋予`docker`权限即可：

```console
sudo chmod a+rw /var/run/docker.sock
```

`docker`已经安装完成，可以使用了。
