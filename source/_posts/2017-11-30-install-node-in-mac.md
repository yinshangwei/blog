---
title: 如何正确的安装nvm、node(mac)
date: 2017-11-30 16:26:43
tags:
- nvm
- node
categories:
- Tools
---


![image](/img/2017-11-30-nvm.png)
## 前言
这里主要介绍mac的安装（linux可以参照），windows系统的话可以参考[windows安装](https://cnodejs.org/topic/5338c5db7cbade005b023c98)。

在mac中我们经常会使用homebrew安装一些软件，但是使用homebrew安装nvm`brew install nvm`后可能会出现错误：**nvm is not compatible with the npm config "prefix" option**。

至于为什么用homebrew安装会出现这个问题，可以参考github上的一个[issue](https://github.com/creationix/nvm/issues/855)。
使用homebrew安装nvm已经是官方不推荐的做法，在nvm的[github文档](https://github.com/creationix/nvm#installation)上也有明确的说明：
>Homebrew installation is not supported. If you have issues with homebrew-installed nvm, please brew uninstall it, and install it using the instructions below, before filing an issue.

<!-- more -->

## 安装步骤
一、**卸载nvm、node**
如果你使用了homebrew安装过nvm和node，请先卸载掉。
`brew list` #查看brew本地安装包
`brew uninstall nvm` #卸载nvm
`brew uninstall node` #卸载node

二、**卸载已安装到全局的npm、node**
`sudo rm -rf /usr/local/lib/node_modules` #删除全局node_modules目录
`sudo rm /usr/local/bin/node` #删除node
`cd  /usr/local/bin && ls -l | grep "../lib/node_modules/" | awk '{print $9}'| xargs rm` #删除全局node模块注册的软链

三、**安装nvm**
`curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.6/install.sh | bash` 或者
`wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.6/install.sh | bash`
安装完成之后可以执行`nvm --version`查看nvm版本号

四、**安装node**
`nvm install node` #安装最新版node
`nvm use node` #指定使用最新版node
`nvm alias default <version>` #指定node的默认版本号
`nvm list` #查看nvm的安装信息（也可以用nvm ls替代nvm list）

五、**常用命令**
`nvm ls-remote` #列出远程服务器所有的node版本
`nvm ls` #列出本地所有安装的node版本
`nvm install <version>` #安装指定版本
`nvm install  --lts` #安装最新稳定版node
`nvm uninstall <version>` #删除已安装的指定版本
`nvm use <version>` #切换使用指定的node版本
`nvm use --lts` #指定使用最新稳定版node
`nvm current` #显示当前的node版本
`nvm alias <name> <version>` #给不同的版本号添加别名
`nvm unalias <name>` #删除已定义的别名
`nvm reinstall-packages <version>` #在当前版本node环境下，重新全局安装指定版本号的npm包
`npm ls -g --depth=0` #查看已经安装在全局的模块
