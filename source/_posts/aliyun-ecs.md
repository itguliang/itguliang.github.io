---
title: 阿里云服务ECS折腾小记（Ubuntu+Node+Mongodb+pm2+Nginx）
date: 2018-10-30
categories:
  - IT
toc: true
copyright: true
abbrlink: a0ad69e3
---
## 相关概念
- **地域和可用区**：指ECS实例所在的物理位置。
- **实例**：等同于一台虚拟机，包含CPU、内存、操作系统、网络、磁盘等最基础的计算组件。
- **实例规格**：指实例的配置，包括vCPU核数、内存、网络性能等。实例规格决定了ECS实例的计算和存储能力。
- **镜像**：指ECS实例运行环境的模板，一般包括操作系统和预装的软件。操作系统支持多种Linux发行版本和不同的Windows版本。
- **块存储**：包括基于分布式存储架构的 云盘和共享块存储，以及基于物理机本地硬盘的 本地存储。
- **快照**：指某一个时间点上一块弹性块存储的数据备份。
- **网络类型**：
  - **专有网络**：基于阿里云构建的一个隔离的网络环境，也称为VPC，VPC之间逻辑上彻底隔离。更多信息，请参考 专有网络VPC。
  - **经典网络**：统一部署在阿里云公共基础设施内，规划和管理由阿里云负责。
- **安全组**：由同一地域内具有相同保护需求并相互信任的实例组成，是一种虚拟防火墙，用于设置实例的网络访问控制。

## 我的配置
配置：1核 2GB 操作系统： Ubuntu 16.04 64位
域名：itguliang.com

## 命令行连接服务器

```bash
$ ssh root@ip #输入密码
```

## 搭建环境

### 安装 node

```bash
$ curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
$ sudo apt-get install -y nodejs
$ node -v   #v10.12.0
$ npm -v    #6.4.1
```

### 安装 mongodb

https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/

导入包管理系统使用的公钥
```bash
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
```
为MongoDB创建一个列表文件
根据Ubuntu 16.04版本创建/etc/apt/sources.list.d/mongodb-org-3.4.list 列表文件
```bash
$ echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
```
更新本地包数据库
```bash
$ sudo apt-get update
```
安装最新版本的MongoDB
```bash
$ sudo apt-get install -y mongodb-org
$ mongo --version #MongoDB shell version v4.0.3
```
配置文件mongod.conf所在路径: /etc/mongod.conf
```bash
# mongod.conf

# for documentation of all options, see:
#   http://docs.mongodb.org/manual/reference/configuration-options/

# Where and how to store data.
storage:
  dbPath: /var/lib/mongodb  #数据库存储路径
  journal:
    enabled: true
#  engine:
#  mmapv1:
#  wiredTiger:

# where to write logging data.
systemLog:
  destination: file
  logAppend: true  #以追加的方式写入日志
  path: /var/log/mongodb/mongod.log  #日志文件路径

# network interfaces
net:
  port: 27017
  bindIp: 127.0.0.1   #绑定监听的ip 127.0.0.1只能监听本地的连接，可以改为0.0.0.0

# how the process runs
processManagement:
  timeZoneInfo: /usr/share/zoneinfo

#security:
#operationProfiling:
#replication:
#sharding:
## Enterprise-Only Options:
#auditLog:
#snmp:
```

启动关闭命令
```bash
$ sudo service mongod start  # 启动
$ sudo service mongod stop   # 关闭
```
卸载安装的软件包
```bash
$ sudo apt-get purge mongodb-org*
```
移除数据库和日志文件（数据库和日志文件的路径取决于/etc/mongod.conf文件中的配置)
```bash
$ sudo rm -r /var/log/mongodb
$ sudo rm -r /var/lib/mongodb
```

mongodb的默认开启端口是27017  安全组规则添加
<!-- - 设置开机启动： -->

### 安装 git
```bash
$ sudo apt-get install git
$ git --version  # git version 2.7.4
```


### pm2 部署

命令行可以启动并公网访问，但是关掉命令行服务就停止了，这个时候需要用到pm2

安装
```bash
$ npm install pm2 -g
```
启动
```bash
$ pm2 start xxx.js #可以配置到package.json
```

设置重启服务器也可以自动启动node
```bash
$ pm2 start   #启动服务
$ pm2 save    #保存当前已经启动了的服务
$ pm2 startup #设置开机自启的配置) 
$ pm2 startup #以后会得到以下提示 设置环境变量
[PM2] Init System found: upstart
[PM2] To setup the Startup Script, copy/paste the following command:
sudo env PATH=$PATH:/opt/bitnami/nodejs/bin /opt/bitnami/nodejs/lib/node_modules/pm2/bin/pm2 startup upstart -u bitnami --hp /home/bitnami

粘贴复制 sudo env….这一部分的命令 执行命令 完成
设置完成，sudo reboot 手动重启服务器pm2 list 查看验证
```
https://blog.csdn.net/softwarenb/article/details/80269660


## 域名访问

首先域名要备案，然后解析对应为公网IP，服务器上部署的项目访问端口默认为80，即可域名访问。


### 安装 Nginx

Nginx是一款轻量级的Web 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器，并在一个BSD-like 协议下发行。其特点是占有内存少，并发能力强，事实上nginx的并发能力确实在同类型的网页服务器中表现较好，中国大陆使用nginx网站用户有：百度、京东、新浪、网易、腾讯、淘宝等。

Nginx中文文档:http://www.nginx.cn/doc/

安装依赖：
```bash
# 安装PCRE，PCRE(Perl Compatible Regular Expressions)是一个Perl库，包括 perl 兼容的正则表达式库。
# nginx的http模块使用pcre来解析正则表达式，pcre-devel是使用pcre开发的一个二次开发库。nginx也需要此库。
$ sudo apt-get install libpcre3 libpcre3-dev

# 安装zlib，zlib库提供了很多种压缩和解压缩的方式，nginx使用zlib对http包的内容进行gzip。
$ sudo apt-get install zlib1g-dev

# OpenSSL ，OpenSSL是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及SSL协
# 议，并提供丰富的应用程序供测试或其它目的使用，nginx不仅支持http协议，还支持https（即在ssl协议上传输http）
$ sudo apt-get install openssl libssl-dev
```

安装nginx：
```bash
$ sudo apt-get update
$ sudo apt-get install nginx
$ nginx -v  # nginx version: nginx/1.10.3 (Ubuntu)
```
安装好的文件位置：

/usr/sbin/nginx：主程序
/etc/nginx：存放配置文件
/usr/share/nginx：存放静态文件
/var/log/nginx：存放日志

Linux系统的配置文件一般放在/etc，日志一般放在/var/log，运行的程序一般放在/usr/sbin或者/usr/bin。

可以看到/etc/nginx/nginx.conf 配置 include /etc/nginx/conf.d/*.conf;
所以可以在/etc/nginx/conf.d 文件夹下自建个配置，自己命名itguliang.conf
```bash
server {
  listen 80;
  server_name www.itguliang.com  itguliang.com;
  location / {
    proxy_pass http://127.0.0.1:3000/;
  }
}
```
然后执行下面命令：
```bash
$ nginx -t #查看配置是否成功
# nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
# nginx: configuration file /etc/nginx/nginx.conf test is successful
$ systemctl start nginx #启动nginx 浏览器输入www.itguliang.com 即可看到Welcome to nginx!字样，如果启动了应用服务，则展示对应界面
$ systemctl stop nginx  #停止nginx
```
开机启动和停止：
```bash
$ sudo systemctl enable nginx #开启 Nginx 随系统一起启动
$ sudo systemctl disable nginx #关闭 Nginx随系统启动
```

### 配置https

阿里云有个单域名免费证书，申请可自动授权系统自动添加TXT解析记录，自动完成域名授权验证，然后下载解压，在/etc/nginx下新建目录cert，然后把crt和key文件上传到cert目录下，配置之前的conf文件如下：
```javascript
server {
   listen 80;
   server_name itguliang.com www.itguliang.com;
   rewrite ^(.*)$ https://www.itguliang.com permanent; 
   #http://itguliang.com 和 http://www.itguliang.com 都会被重定向到 https://www.itguliang.com
}
server {
   listen 443;
   server_name itguliang.com www.itguliang.com;
   ssl on;
   ssl_certificate   /etc/nginx/cert/cert-1541737820369_www.itguliang.com.crt;
   ssl_certificate_key /etc/nginx/cert/cert-1541737820369_www.itguliang.com.key;

   location / {
     proxy_pass http://127.0.0.1:3000/;
   }
}
```

## 常用命令
```bash
$ pm2 restart
$ pm2 list
$ pm2 reload all # 立即重启所有工作进程
$ pm2 stop all  # 停止所有应用
$ locate mongo  #查看mongodb的安装位置
$ pgrep mongo -l #查看是否已经启动mongodb
```


## 遇见问题
** Q: Ubuntu保存退出vim编辑器 **
保存退出：按“Esc”键后 此时的“插入”会消失，然后按Shift+zz 就可以保存修改内容并退出
不保存退出：当修改修改了一部分内容后发现修改错了，此时就会进行不保存退出，按“Esc”键后，再输入“：”之后在输入命令时直接输入“q!” 
强制退出：  按“Esc”键后，再输入“：”之后在输入命令时直接输入“!”

** Q: mongodb connect failed **
```bash
#找到 mongod.lock 的位置 
$ locate mongod.lock
#输出  /var/lib/mongodb/mongod.lock

#之后 cd /var/lib/mongodb/
$ rm mongod.lock
$ rm -r _tmp

#也可以尝试着修复下
$ mongo -repair

#也可以查看一下日志 
$ cat  /var/log/mongodb/mongodb.log
```