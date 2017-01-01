---
layout: post
title: Ubuntu 下使用 update-alternatives 管理 Lua 版本
issueid: 5
categories: 
- Linux
tags:
- Linux
- Lua
- update-alternatives
---

Ubuntu 安装 Lua 当前默认版本为 Lua5.1，如果我们想使用 Lua5.3 的话，可以使用以下命令安装。

    sudo apt install lua5.3

但是安装后的命令为 lua5.3 并不是我们希望的 lua，lua 默认启动的是 Lua5.1 版本，如果我们只安装 Lua5.3 的话，比如在源码安装 vim 使其支持 Lua 时会提示找不到 Lua，这里有几个解决方法：


1. 安装 Lua5.1
2. 源码编译安装 Lua5.3
3. 使用 update-alternatives

我用的是第 3 种方法，运行以下命令 

    sudo update-alternatives --install /usr/bin/lua lua /usr/bin/lua5.3
    sudo update-alternatives --install /usr/bin/luac luac /usr/bin/luac5.3

之后我们可以使用 lua 命令了

    lua 指向 /usr/bin/lua
    /usr/bin/lua 指向 /etc/alternatives/lua
    /etc/alternatives/lua 指向 /usr/bin/lua5.3

和使用 lua5.3 启动是一样的效果。

![update-alternatives](/images/update-alternatives.png)

通过这个例子，我们也可以使用 update-alternatives 工具来管理 Python 的版本，很多机器上同时安装了 2.x 和 3.x 两种版本，之前在 V 站（还是知乎？记不清了）上看到有人问怎么处理才能使 python 命令启动 python3.x 而不是 python2.x，当然解决的方法很多，改 .bashrc，建立链接什么的都可以，但是如果使用工具的话可能会更加方便些，这才是使用工具的目的。

所以，有多个版本的命令我们可以使用 update-alternatives 工具来管理，这样的命令还有很多，比如 pip 的版本，Java 的版本等等。

如果想进一步了解 update-alternatives 工具，可以使用以下命令查看

    man update-alternatives
