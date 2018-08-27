---
title: 记一次向开源项目提交PR的过程
date: 2018-08-08 09:32:40
categories: "Notes"
tags:
     - 开源
     - 成长之路
comments: true
---

> 最近在做Electron+Vue的项目，这里用到了[这个项目](https://github.com/PanJiaChen/electron-vue-admin)作为脚手架。然而，在准备打包生产环境配置，用于发布第一个正式版本的时候，发现把`process.env.NODE_ENV`设置为production并不能切换为生产环境的配置。

#### 原因
经排查，发现问题的原因是因为配置文件中Webpack的DefinePlugin插件把配置写死为开发环境了。
```javascript
'process.env': config.dev.env
```
这就导致了不能根据当前设置的环境进行全局配置切换，故使用一个简单的三元运算符可以解决
```javascript
'process.env': process.env.NODE_ENV === 'production' ? config.build.env : config.dev.env`。
```
**上Github看了项目主干最新代码，发现此问题并没有修复，本着同性交友的原则，我准备提交我的第一个PR。**
<!-- more -->

#### 步骤
> fork

把要提交PR的项目fork到自己的仓库，在开源项目的右上角点击fork，稍后片刻即可

![](https://ws4.sinaimg.cn/large/006tNbRwgy1fun1rmxvhgj30oo03ymxk.jpg)

进入到自己fork的项目中，可以看到看到`Clone or download`按钮，如下，通过这个链接把代码clone到本地

![](https://ws2.sinaimg.cn/large/006tNbRwgy1fun1uz1g16j30ns0dc0v0.jpg)

> clone

使用git clone命令把代码克隆到本地仓库
```shell
git clone https://github.com/***/***.git
```
代码clone完成后进入到项目目录，输入`git status`命令，可以看到当前正在master分支上
```shell
$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```

再输入`git remote  -v`命令，可以看到此时只与自己的远程仓库建立了连接

```shell
$ git remote -v
origin	https://github.com/LuoLiangDSGA/electron-vue-admin.git (fetch)
origin	https://github.com/LuoLiangDSGA/electron-vue-admin.git (push)
```

所以还需要与一开始fork的那个项目建立连接，执行如下命令：

```shell
git remote add upstream https://github.com/PanJiaChen/electron-vue-admin.git
```
再使用`git remote -v`可以看到：
```shell
$ git remote -v
origin	https://github.com/LuoLiangDSGA/electron-vue-admin.git (fetch)
origin	https://github.com/LuoLiangDSGA/electron-vue-admin.git (push)
upstream	https://github.com/PanJiaChen/electron-vue-admin.git (fetch)
upstream	https://github.com/PanJiaChen/electron-vue-admin.git (push)
```
> 创建分支

```shell
git checkout -b electron-vue-admin-fixbug
Switched to a new branch 'electron-vue-admin-fixbug'
```
分支创建之后会自动切换到该分支，修改代码的步骤省略

> 提交

```shell
$ git commit -am ":bug: fix package bug"
$ git push --set-upstream origin electron-vue-admin-fixbug
```

> 创建并提交PR

此时在fork的仓库中可以看到刚才的提交记录，选择`New pull request`创建PR

![](https://ws3.sinaimg.cn/large/006tNbRwgy1fun2f9eszvj30s807e0ty.jpg)

写好名字和说明提交即可，PR提交后，项目拥有者会进行判断，选择是否合进主干。我提交PR的项目响应非常快，几个小时便合进了主干，如下：

![](https://ws4.sinaimg.cn/large/006tNbRwgy1fun2lne59xj31kw107dqu.jpg)

交友成功！

#### 最后
虽然是一个很小的bug，但是丰富了自己的经历，交到了朋友，感谢开源，让我成长。