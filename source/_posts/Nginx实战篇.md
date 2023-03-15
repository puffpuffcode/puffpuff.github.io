---
title: Nginx实战篇
date: 2023-01-12 07:44:59
author:
tags: Nginx
categories: 
post_link:
description:
header:
mathjax:
copyright:
sticky:
reward_setting:
---
<!--more-->
# 使用 Nginx 的正确姿势

> 运行环境：Fedora 34


## 环境准备 & 安装
```sh
[root@fedora34 ~]# dnf install nginx
```

### 启动 Nginx
```sh
[root@fedora34 ~]# systemctl start nginx.service
```

### 查看 Nginx 状态

```sh
[root@fedora34 ~]# systemctl status nginx.service
● nginx.service - The nginx HTTP and reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; vendor preset: disabled)
     Active: active (running) since Thu 2023-01-12 15:50:57 CST; 1s ago
    Process: 2746392 ExecStartPre=/usr/bin/rm -f /run/nginx.pid (code=exited, status=0/SUCCESS)
    Process: 2746393 ExecStartPre=/usr/sbin/nginx -t (code=exited, status=0/SUCCESS)
    Process: 2746394 ExecStart=/usr/sbin/nginx (code=exited, status=0/SUCCESS)
   Main PID: 2746395 (nginx)
      Tasks: 2 (limit: 2333)
     Memory: 4.4M
        CPU: 63ms
    # 有几个 worker 就表示服务器有几个核心数，可以在配置文件中配置
     CGroup: /system.slice/nginx.service
             ├─2746395 nginx: master process /usr/sbin/nginx
             └─2746396 nginx: worker process
```
可以看到 Nginx 已经成功启动。

跳转到服务器地址，出现此页面则为成功配置运行。

![图片失效咯](/images/nginx_success.png)

### 将 Nginx 服务设置为每次开机启动

```sh
[root@fedora34 ~]# systemctl enable nginx
Created symlink /etc/systemd/system/multi-user.target.wants/nginx.service → /usr/lib/systemd/system/nginx.service.
```

## 常用命令
```sh
systemctl enable nginx # 将 nginx 作为服务
systemctl status nginx # 查看 nginx 状态
nginx -v # 查看 nginx 版本
nginx -V # 查看 nginx 版本以及配置选项
```

## 简单配置
首先进入 Nginx 配置目录下，并且新建一个配置文件 `hello.conf`，填入以下配置。

```sh
[root@fedora34 /]# cd /etc/nginx/conf.d/
[root@fedora34 conf.d]# vim hello.conf

server{
        listen 81;
        location / {
                root /home/;
                index index.htm index.html;
        }
}
```

完成后使用 `nginx -t` 测试配置文件是否正确。
```sh
[root@fedora34 conf.d]# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

通过测试后重新加载配置文件 `nginx -s reload` 。

---
先写到这，发现得先去补一下正则的知识。