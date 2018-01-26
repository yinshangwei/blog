---
title: 如何搭建自己的专属ShadowSocks
date: 2018-01-25 11:07:30
tags:
- ShadowSocks
categories:
- Tools
---


![image](/img/2018-01-25-shadowsocks.png)
## 购买VPS服务器
如果你想拥有自己的专属网络用来科学上网，首先你需要购买一台VPS服务器。
市面上这样的产品也非常多，可以参考[VPS优惠信息](http://www.zrblog.net/category/vps-2/offers)。
VPS服务器的架构一般有两种[OpenVZ和KVM](https://clientarea.ramnode.com/knowledgebase.php?action=displayarticle&id=52)，建议选择KVM服务器，比较稳定并且可定制化程度更高。

<!-- more -->


## 配置VPS服务器
购买服务器后，一般会收到关于服务器的基本信息（ip address, username, password等）。
然后通过`ssh username@ip_address`命令进入到我们的服务器，需要输入password。
首次进入到服务器后，建议通过`passwd`命令来修改初始password。
如果不想每次ssh进入服务器的时候都输入密码，可以把本机`.ssh/id_rsa.pub`公钥写入到服务器的`.ssh/authorized_keys`中（没有该文件就创建），然后修改本机的`.ssh/config`文件。
```text
Host *
 AddKeysToAgent yes
 UseKeychain yes
 IdentityFile ~/.ssh/id_rsa

Host server_name
 Hostname ip_address
 Port port
 User username
```
完成之后，可以直接输入`ssh server_name`(不需要输入ip address)进入到服务器而不需要输入密码。


## 安装ShadowSocks
#### 什么是ShadowSocks
> ShadowSocks是一种socks5的代理，而socks5的代理服务器则是把你的网络数据请求通过一条连接你和代理服务器之间的通道，由服务器转发到目的地。你没有加入任何新的网络，只是http/socks数据经过代理服务器的转发送出，并从代理服务器接收回应。

#### 下载并安装shell脚本
```bash
wget --no-check-certificate -O shadowsocks.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh
chmod +x shadowsocks.sh
./shadowsocks.sh 2>&1 | tee shadowsocks.log
```
* 输入ShadowSocks密码，需要记住
* 输入端口号，默认为`8989`
* 选择加密方式，默认为`aes-256-gcm`，建议修改为`7）aes-256-cfb`

#### 常用命令
> 启动：/etc/init.d/shadowsocks start
  停止：/etc/init.d/shadowsocks stop
  重启：/etc/init.d/shadowsocks restart
  状态：/etc/init.d/shadowsocks status
  卸载：./shadowsocks.sh uninstall
  
#### 配置文件
> 配置文件路径：/etc/shadowsocks.json

* 单用户配置：
```json
{
    "server":"0.0.0.0",
    "server_port":8989,
    "local_address":"127.0.0.1",
    "local_port":1080,
    "password":"password",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```

* 多用户配置：
```json
{
    "server":"0.0.0.0",
    "local_address":"127.0.0.1",
    "local_port":1080,
    "port_password":{
         "8989":"password0",
         "9001":"password1",
         "9002":"password2"
    },
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```


## 安装ShadowSocks客户端
* [Windows ShadowSocks客户端](https://github.com/shadowsocks/shadowsocks-windows/releases)

* [Mac OSX ShadowSocks客户端](https://github.com/shadowsocks/ShadowsocksX-NG/releases)

* [Android ShadowSocks客户端](https://github.com/shadowsocks/shadowsocks-android/releases)

* IOS ShadowSocks客户端（在Apple Store搜索wingy，如FirstWingy、SuperWingy等都可以）
