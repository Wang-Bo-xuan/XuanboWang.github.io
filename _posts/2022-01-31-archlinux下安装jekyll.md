---
title: arch linux下安装jekyll 
date: 2022-01-31 19:32:00 +0800
categories: [archlinux, jekyll]
tags: archlinux jekyll
---

`AUR`已经有了`jekyll`，可以直接安装，但是本文还是依旧从`rubygems`开始

首先使用如下命令安装`rubygems`

```console
sudo pacman -S rubygems
```

安装`jekyll`前需要更新`gem`组件，使用如下命令即可

```console
gem update
```

`gem`组件更新完成后，就可以使用如下命令安装`jekyll`及`bundler`

```console
gem install jekyll bundler
bundle update
```

在`gem`安装时可能出现`undefined method 'size' for nil:NilClass(NoMethodError)`问题，可以参考如下方案

在`.bashrc`文件内加入如下两个变量

```bash?linenums
export GEM_HOME="$(ruby -e 'puts Gem.user_dir')"
export PATH="$PATH:$GEM_HOME/bin"
```

然后使用如下命令查看`gem`环境

```console
gem env
```

得到gem的PATH路径，比如

```bash?linenums
- GEM PATHS:
  - /home/wbx/.local/share/gem/ruby/3.0.0
  - /usr/lib/ruby/gems/3.0.0
```

将这两个目录下的cache目录删除，再次执行`gem`安装及更新时就不会出错了
