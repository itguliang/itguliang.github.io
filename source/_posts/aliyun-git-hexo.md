---
title: 阿里云搭建私人git服务器、一键部署Hexo博客
date: 2019-06-27
categories:
  - IT
toc: true
copyright: true
abbrlink: 3ceebd6f
---
服务器操作系统：Ubuntu 16.04 64位

### 1、安装git

可以执行 git --version 检查是否安装了Git
```bash
$ sudo apt-get install git # ubuntu
$ yum install git # centos
$ git --version  # 提示 git version 2.7.4 则安装成功
```

### 2、创建git用户及权限
首先创建一个用户组，建立用户组的目的在于对于这个git服务器，赋予多人访问权限时，可以统一管理。
```bash
$ groupadd git
$ tail -n 2 /etc/group
# .....
# 输出类似 git:x:1000: 的提示
```

在用户组git下创建一个用户，名字为 itguliang
```bash
$ useradd -d /home/itguliang  -g git -m itguliang 
#  home文件夹下会自动创建itguliang文件夹
$ passwd  username # 修改密码 git提交等命令会需要
$ id itguliang
# 输出类似 uid=1000(itguliang) gid=1000(git) groups=1000(git)
```

### 3、在客户端创建RSA密钥

(自己电脑进行操作)

```bash
$ ssh-keygen 
# 输入命令提示Generating public/private rsa key pair.
# Enter file in which to save the key (/Users/。。。/.ssh/id_rsa):
```
按回车会生成 ~/.ssh/id_rsa私钥和 ~/.ssh/id_rsa.pub 公钥这两个文件。如果提示已经存在，那就直接把 ~/.ssh/id_rsa.pub  这个文件里的内容全部复制下来，然后进行下一步。

### 4、在服务器上建立文件保存公钥

(服务器上进行操作)

```bash
$ cd /home/itguliang/
$ mkdir .ssh
$ chmod 700 .ssh  #只有拥有者有读、写、执行权限。
$ touch .ssh/authorized_keys
$ chmod 600 .ssh/authorized_keys #只有拥有者有读写权限
$ chown -R itguliang:git .ssh  #用户名：组名
```

其中/home/itguliang目录为服务器上用户itguliang 的主页目录，上述操作相当于在/home/itguliang/.ssh/目录下新建一个 authorized_keys 文件。并把目录  .ssh  的权限设置为700，authorized_keys 文件权限设置为600.
因为git的pull相当于读操作，push相当于写操作，所以需要读写权限。
```bash
$ vi authorized_keys
# esc :wq 保存文件并退出vi 相关编辑命令自行百度
```

### 5、在服务器初始化git仓库
```bash
$ cd /project
$ mkdir git
$ chown itguliang:git git/
$ cd git
$ git init --bare hexo-blog.git
$ chown -R itguliang:git hexo-blog.git
```

### 6、本地克隆

```bash
$ git clone itguliang@XXX.XXX.XXX.XXX:/project/git/hexo-blog.git
```

会提示你输入git的密码，输入进去，然后会再提示: You appear to have cloned an empty repository.
这说明服务器已经OK了。