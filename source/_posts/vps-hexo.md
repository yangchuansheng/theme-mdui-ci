---
title: 失踪人口回归系列之在VPS上搭建HEXO的GIT部署环境
date: 2017/03/22 17:53:00
categories:
- 技术
tags:
- Hexo
- 教程
---
失踪人口回归系列之第一篇
<!--more-->
## 写在开头
由于HEXO是静态博客，越来越多的人选择在GitHub, Coding等Pages服务提供商处搭建自己的博客，从而省下买网页空间或VPS的钱。

然而有部分人会选择在VPS上使用HEXO，但是却发现远不如使用Pages服务方便，但看完这篇文章，你就会发现，这并没有想象中那么~~方便~~难。

## 准备工作
- ~~一台电脑~~
- ~~一台VPS~~
- ~~一个打开本页面的浏览器~~

### 本地环境
#### 现在本地上安装Node.js和git
请访问[Installing Node.js via package manager](https://nodejs.org/en/download/package-manager/)，安装你系统的对应版本。
#### 安装Hexo
```` bash
$ npm i -g hexo-cli
````
> 如果你有`cnpm`, 只需要将`npm`替换成`cnpm`

#### 创建Hexo
```` bash
$ mkdir /path/to/hexo
$ cd /path/to/hexo
$ hexo init
$ hexo s
````
> 其中`/path/to/hexo`为你想要存放Hexo数据的地方

如果`hexo init`过程中安装终端，请执行
```` bash
$ cd /path/to/hexo
$ npm i
````
打开[localhost:4000](localhost:4000), 如若看见Hexo初始页面则Hexo安装成功。

#### 生成SSH公钥密钥
> 由于我曾经进过此步发现并没有什么用，故而以下内容皆为引用自[Viosey](https://blog.viosey.com/2016/10/05/hexo-vps-git/#生成-SSH-公钥密钥)。
> 经实测此步可忽略，区别在于`hexo d`过程是否需要输入密码。

```` bash
$ cd ~/.ssh
$ ssh-keygen
````
它先要求你确认保存公钥的位置（.ssh/id_rsa），然后它会让你重复一个密码两次，如果不想在使用公钥的时候输入密码，可以留空。

[GitHub Help - Generating a new SSH key and adding it to the ssh-agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)

### 服务器环境

#### 安装Git & Node.js

##### Ubuntu
```` bash
$ apt-get install git
$ curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
$ apt-get install -y nodejs
````
此为 Node.js v6

若想安装其他版本，请更改为
Node.js v7
```` bash
$ curl -sL https://deb.nodesource.com/setup_7.x | sudo -E bash -
````

##### CentOS
```` bash
$ curl -sL https://rpm.nodesource.com/setup | bash -
$ yum install -y nodejs
````
此为 Node.js 0.10

若想安装其他版本，请更改为
Node.js v7
```` bash
$ curl --silent --location https://rpm.nodesource.com/setup_7.x | bash -
````

Node.js v6 LTS
```` bash
$ curl --silent --location https://rpm.nodesource.com/setup_6.x | bash -
````

#### 创建 git 用户 & 禁用 git 用户的 shell 登录权限
```` bash
$ adduser git
$ vim /etc/passwd
````
将
```` bash
git:x:1001:1001:,,,:/home/git:/bin/bash
````
改为
```` bash
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
````

#### 添加证书登录
##### 已生成公钥
把`.pub`公钥里的内容添加到服务器的`/home/git/.ssh/authorized_keys`文件中

##### 未生成公钥
请执行在服务器端执行
```` bash
$ passwd git
````
更改git的密码。

#### 建立git仓库
```` bash
$ mkdir /path/to/repo
$ cd /path/to/repo
$ git init --bare hexo.git
````
目录可自行选择。
eg.
```` bash
$ mkdir /home/git/repos
$ cd /home/git/repos
$ git init --bare hexo.git
````
然后在`/home/git/repos`下会有一个`hexo.git`的文件夹
> 使用 --bare 参数，Git 就会创建一个裸仓库，裸仓库没有工作区，我们不会在裸仓库上进行操作，它只为共享而存在。

#### 配置git hooks
在`hexo.git/hooks`目录下，执行
```` bash
$ cd /path/to/hexo.git/hooks
$ nano post-receive
````
> 你可以选择自己熟手的编辑器新建这个`post-receive`

在`post-receive`中写入如下内容:
```` bash
#!/bin/sh
git --work-tree=/path/to/your/site --git-dir=/path/to/hexo.git checkout -f
````
其中`/path/to/your/site`为你的站点文件夹，`/path/to/hexo.git`为你所建立`hexo.git`的完整路径

eg.
```` bash
#!/bin/sh
git --work-tree=/var/www/blog.halyul.com --git-dir=/home/git/repos/hexo.git checkout -f
````

设置`post-receive`的可执行权限
```` bash
$ chmod +x post-receive
````

#### 设置目录拥有者
```` bash
$ chown -hR git:git /path/to/hexo.git
$ chown -hR git:git /path/to/your/site
````

## 本地Hexo配置
先安装`hexo-deployer-git`以执行hexo的git部署
```` bash
$ npm i --save hexo-deployer-git
````

打开hexo目录下的`_config.yml`，找到`deploy`进行修改
```` yaml
deploy:
    type: git
    repo: git@example.com:hexo.git
    branch: master
````
> 其中`example.com`为你的站点地址或IP

## 开始使用
当你需要生成以及部署hexo时，运行
```` bash
$ hexo clean && hexo d
````

**大功告成~**

## 参考
[在 VPS 上搭建 Hexo 博客，使用 Git 部署](https://blog.viosey.com/2016/10/05/hexo-vps-git/)
