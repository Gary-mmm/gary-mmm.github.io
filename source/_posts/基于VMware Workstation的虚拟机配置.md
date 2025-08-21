---
title: 基于VMware Workstation的虚拟机配置
date: 2025-08-02 09:33:40
tags: tech
---
身为网安学生必然会用到虚拟机。
## MacOS
和当年不一样，现在在mac上能玩的游戏越来越多了。拿mac打游戏也未必是精神病（

不过虚拟机偶尔还是要用到的。商业软件Parallels Desktop是要花米的，不过性能上可能好一点。免费的可以用基于QEMU的UTM，或者VMware Fusion。

使用体验：macbook air M2丐版。因为存储不太够所以已经不怎么用了。UTM运行windows的时候比较吃性能，基本上没法用。但是装一个linux系统可以流畅运行，不过分辨率不是很高，还得担心偶尔出现bug。

## Windows

在上面打游戏当然没什么问题，不过有时候还是要一个linux系统。wsl和虚拟机都是不错的。这里用VMware Workstation实现Ubuntu22.04的安装。

[Ubuntu22.04下载](https://releases.ubuntu.com/jammy/)选择ubuntu-22.04.5-desktop-amd64.iso即可
[VMware Workstation下载网址](https://support.broadcom.com/group/ecx/productdownloads?subfamily=VMware%20Workstation%20Pro&freeDownloads=true)此处需要先注册账号，目前已经更新至17.6.4.
点击链接后应该首先阅读条款（就算不读也要点进去），然后勾选已阅读条款，随后不出意外的话就可以点击连接了。
意外就是
>Account verification is Pending. Please try after some time.

可以在网上自行查找解决方法

#### Ubuntu安装
按照默认配置安装即可

```sh
# 更新软件源列表
sudo apt-get update

# 更新软件
sudo apt-get upgrade

# 下载安装 open-vm-tools-desktop
sudo apt-get install open-vm-tools-desktop -y
```

#### 安装docker
```shell
sudo apt install docker-ce docker-ce-cli containerd.io

出现问题如下
E: Package 'docker-ce' has no installation candidate
E: Unable to locate package docker-ce-cli
E: Unable to locate package containerd.io
E: Couldn't find any package by glob 'containerd.io'
```
于是随便找了一篇[教程](https://blog.csdn.net/Cike___/article/details/146415836)

省流如下

确保你的本地软件包列表是最新的，执行命令：
```bash
sudo apt-get update
```
安装必要的软件包，以便可以通过 HTTPS 使用存储库：
```bash
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common

```
添加 Docker 的官方 GPG 密钥：
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

```
设置 Docker 稳定版存储库：
```bash
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

```
安装 Docker 引擎：
```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io

```
验证安装是否成功

运行一个简单的Docker命令来验证是否正确安装了Docker引擎：
```bash
sudo docker run hello-world

```

最后一步会有一点问题：
> Unable to find image 'hello-world:latest' locally
docker: Error response from daemon: Get "https://registry-1.docker.io/v2/": context deadline exceeded

> Run 'docker run --help' for more information

于是加一些镜像源[参考](https://blog.csdn.net/oyjl__/article/details/143522664)
```

```

