---
title: git和github的使用
date: 2020-03-05 09:52:07
categories: 前端
tags: ['git','github']
---

git是一个代码版本管理工具，git可以创建本地和远程仓库，其中github就可以作为远程代码仓库。windows中要使用git需要安装git bash。
### git bash
git bash是一套linux虚拟环境，内置linux命令和git命令。它是比cmd更好用的命令行工具。
安装完成后首先全局设置好用户名，用户邮箱等属性。
```
npm config set registry https://registry.npm.taobao.org
git config --global user.name xxx #设置username
git config --global user.email xxx #设置邮箱
git config --global push.default simple # 
git config --global core.quotepath false #防止文件名变成数字
git config --global core.editor "vim" # 使用vim编辑提交信息
```
### 本地仓库
1. 初始化仓库 git init
```
cd <path>
mkdir <dirName>
cd <dirName>
git init    #初始化仓库
```

2. git add [-A /-u/./<path>] 
添加文件或者文件夹到本地仓库暂存区（有些地方也称为索引库）。
```
    git add <path>  添加指定的文件和文件夹
    git add .       添加仓库中所有变动到暂存区，但**不包括删除的文件**
    git add -u      添加已经tracked的文件变动至暂存区，但不包括**新添加的文件**
    git add -A      添加所有文件和变动至暂存区
```

3. git commit [<-m description>]
将暂存区的内容提交到本地仓库。如果不添加-m参数，那么会自动打开vim编辑器让我们输入commit描述。
vim编辑器通过<kbd>i</kbd>来切换编辑模式，编辑完成后通过<kbd>esc</kbd>来切换命令模式，并使用<kbd>:wq</kbd>保存退出或者使用<kbd>:q!</kbd>不保存直接退出。

### 远程仓库
1. 远程仓库创建：直接在github上create repository
2. 关联远程仓库
```
    git remote add origin git@github.com:XXX/YY.git     # ssh关联
    git remote add origin https://github.com/XXX/YY.git     # https关联
```

3. 推送到远程仓库
```
    git pull    #当远程库被人为改变导致本地和远程仓库不一致，就需要git pull后再git push
    git push -u origin master 
```

### 其他常用指令
git status <-sb> 查看git状态(显示总结和分支)
git log 查看提交记录
git clone <地址>  克隆代码
git remote set-url origin git@github.com:xxxxx.git 重新设置远程地址
git branch 新建分支
git merge 合并分支
git stash 保存当前进度但不提交，然后可以切换到其他分支
git checkout 切换分支
git stash pop 读取之前保存的未提交记录
git revert 撤销某次提交
git reset 回退到撤销点
git diff 查看详细变化

### 几个文件的作用
.gitignore 忽略提交的文件或者文件夹
README.md  文档的描述
LISENCE 开源协议许可证

### 新的github账号
首次创建github账号或者需要github支持多台设备时，就需要用到SSH绑定。
#### SSH key的获取
windows系统中, 通常"\~/"代表绝对路径"/c/Users/Administrator"（或者说当前用户user的绝对位置）。在"~/.ssh/" 中有两个文件，id_rsa相当于钥匙，id_rsa.pub相当于锁。我们只需要复制id_rsa.pub到github的SSH key中即可。
可以通过以下命令来获取新的SSH key：
```
 rm -rf ~/.ssh/*    #删除原来的key
 ssh-keygen -t rsa -b 4096 -C "你的邮箱"    # 连按三次enter，生成新的key
```
与github绑定后，通过```ssh -T git@github.com```来验证是否绑定成功。