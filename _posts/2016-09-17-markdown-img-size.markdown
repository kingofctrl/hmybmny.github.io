---
layout: post
title: Markdown 语法控制图片大小
issueid: 2
categories: 
- Others
tags:
- Linux 
- Markdown
---

发第一篇文章时最后两张图太大，已经超过了屏幕正常的宽度，当时搜了一些资料，给出的方法基本都是 `<img ...></img>` 的形式，也有给出 markdown 语法的，测试后都不能用。后来准备使用图床，把图片存到 GitHub 仓库得到图片地址后使用 `<img>` 标签控制。之前也想尝试使用七牛，但是没有绑定域名的话只能使用测试域名，最后只好放弃。正好这两天中秋放假，换了个姿势重新找了资料，终于找到了 markdown 控制图片大小的方法，格式如下：

```
![img](img.png){: width="100px" height="100px"}
![img](img.png){: width="100px" height="100px" 其它需要的属性}
```


![命令运行网易云音乐](/images/4.gif)

对于上图来说有以下几种控制方式：

```
![命令运行网易云音乐](/images/4.gif){: width="100px" height="100px"}
效果如下：
```
![命令运行网易云音乐](/images/4.gif){: width="100px" height="100px"}

```
![命令运行网易云音乐](/images/4.gif){: width="200px" height="200px"}
效果如下：
```
![命令运行网易云音乐](/images/4.gif){: width="200px" height="200px"}

```
![命令运行网易云音乐](/images/4.gif){: width="300px" height="300px"}
效果如下：
```
![命令运行网易云音乐](/images/4.gif){: width="300px" height="300px"}

```
![](/images/4.gif){: width="800px" height="400px" alt="命令运行网易云音乐"}
效果如下：
```
![](/images/4.gif){: width="800px" height="400px" alt="命令运行网易云音乐"}

参考资料

1. [https://stackoverflow.com/questions/14675913/how-to-change-image-size-markdown](https://stackoverflow.com/questions/14675913/how-to-change-image-size-markdown)
