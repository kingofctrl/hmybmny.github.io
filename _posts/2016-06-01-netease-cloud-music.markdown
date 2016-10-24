---
layout: post
title: 非 Debian 用户使用官方网易云音乐
issueid: 1
categories: 
- Linux
tags:
- Linux
- Music
---

终于等来了网易云音乐发布 linux 版客户端。之前试过 [FeelUOwn](https://github.com/cosven/FeelUOwn)（最近作者进行了更新），但是在 openSUSE 下有一些问题，我觉得体验较好的还是 [musicbox](https://github.com/darknessomi/musicbox) ，命令行版本，该有的功能基本都有了，用了很长一段时间，现在官方版本发布，先体验一下！由于官方只提供 .deb 包，对于非 Debian 用户来说不能直接使用 .deb 安装，于是有人将其打包为 rpm 格式，方便其它能够使用 rpm 的用户使用， rpm 链接 [netease-cloud-music-rpm](https://github.com/Senorsen/netease-cloud-music-rpm) 。如果你想直接使用官方发布的安装包，那只需要很少的几步操作就能达到要求。

- 本文采用的安装包是网易云音乐官网下载的 Ubuntu 16.04 （64 位）
- 运行环境 openSUSE Leap 42.1 (x86_64) 64 位
- 其它 linux 发行版的用户也可以使用本文的方法，对于提示的错误如果不影响使用可以不用管，否则就需要安装相应的依赖包

找到 .deb 所在目录将 .deb 解压

![解压.deb](/images/1.gif)

解压后执行以下命令将 netease-cloud-music* 文件夹移动到 /opt 文件夹（也可以是其它）

```
sudo cp -r netease-cloud-music_0.9.0-2_amd64 /opt
cd /opt/netease-cloud-music_0.9.0-2_amd64
sudo tar -xvf data.tar.xz
```

![移动文件夹](/images/2.gif)

接下来进就可以运行网易云音乐，这种方法不会引起内存泄漏，所以可以用这种方法避免内存泄漏

```
./usr/bin/netease-cloud-music
```

![运行网易云音乐](/images/3.gif)

如果你觉得不够方便，你可以将以下命令粘贴到 .bashrc ，但是用这种方法会引起内存泄漏，具体原因不清楚

```
alias netease-cloud-music='cd / && \
    ./opt/netease-cloud-music_0.9.0-2_amd64/usr/bin/netease-cloud-music'
```

![命令运行网易云音乐](/images/4.gif)

启动终端后就可以通过命令 netease-cloud-music 启动网易云音乐了
