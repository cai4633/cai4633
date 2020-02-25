---
title: Git error cannot spawn ssh No such file or directory的一个解决办法
date: 2019-10-14 20:30:27
categories: git
tags: 
---

今天在hexo中，部署博客到github上时出现一个错误：
 **Git error** cannot spawn ssh No such file or directory 

这个错误最后发现是**ssh**没有加入到**环境变量**中，将**`"D:\Git\usr\bin\ssh.exe"`**加入到**环境变量**后，问题解决。

