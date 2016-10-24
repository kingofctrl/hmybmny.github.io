---
layout: post
title: Linux 修改镜像源加快下载速度(pip-RubyGems-NPM-Docker)
issueid: 3
categories: 
- Linux
tags:
- Linux
- pip
- RubyGems
- npm
- docker
---

所有配置当前用户和 root 用户最好都修改，否则 root 用户模式下使用的依旧是官方源。

**pip:**

```
mkdir ~/.pip && cd .pip/
echo "[global]">>pip.conf
echo "index-url = https://pypi.tuna.tsinghua.edu.cn/simple">>pip.conf
```

**RubyGems:**

```
# 移除默认源
gem sources --remove https://rubygems.org/  
# 添加清华源
gem sources -a https://mirrors.tuna.tsinghua.edu.cn/rubygems/  
# 查看当前源，确保只有 https://mirrors.tuna.tsinghua.edu.cn/rubygems/
gem sources -l  

```

**NPM:**

在 ~/.npmrc 中添加：

```
registry=http://npmreg.mirrors.ustc.edu.cn/
```

**Docker:**

在 /etc/docker/daemon.json 文件中添加：

```
{
    "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
}
```

**教育网的用户推荐添加清华源或者中科大源，这样速度会快很多，能达到几兆每秒。**

参考资料

1. [开源镜像使用帮助列表](https://lug.ustc.edu.cn/wiki/mirrors/help)
2. [清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/)
