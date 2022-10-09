---
title: rabbitmq
description: 正文被隐藏
copyright: false
cover: 文章图片连接
date: 2022-10-09 11:13:33
tags: rabbitmq安装
categories:
---
## 下载RabbitMQ
> 下载地址：https://www.rabbitmq.com/download.html
## 下载Erlang
> Erlang和RabbitMQ版本对照：https://www.rabbitmq.com/which-erlang.html
> 下载地址：https://packagecloud.io/rabbitmq/erlang/packages/el/7/erlang-23.2.7-2.el7.x86_64.rpm

## 安装Erlang 
```bash
rpm -Uvh erlang-23.2.7-2.el7.x86_64.rpm
```
查看版本号：`erl -v`

## 安装RabbitMQ
1. 安装必要依赖组件
```bash
yum install build-essential openssl openssl-devel unixODBC unixODBC-devel make gcc gcc-c++ kernel-devel m4 ncurses-devel tk tc xz
```
2. 安装socat
```bash
yum install -y socat
```
3. rmp 安装
```bash
 rpm -Uvh rabbitmq-server-3.8.35-1.el8.noarch.rpm
```

## 操作
1. 启动rabbitmq:`systemctl start rabbitmq-server` 
2. 查看状态： `systemctl status rabbitmq-server`
3. 开机自启动：`systemctl enable rabbitmq-server`
4. 关闭rabbitmq:`systemctl stop rabbitmq-server` 
5. 重启rabbitmq:`systemctl restart rabbitmq-server` 


## 管理界面插件
`rabbitmq-plugins enable rabbitmq_management`
rabbitmq有一个默认的账号密码guest，但该情况仅限于本机localhost进行访问，所以需要添加一个远程登录的用户
1. 添加用户：`rabbitmqctl add_user 用户名 密码`
2. 设置用户角色,分配操作权限：`rabbitmqctl set_user_tags 用户名 角色`
3. 为用户添加资源权限(授予访问虚拟机根节点的所有权限)：`rabbitmqctl set_permissions -p / 用户名 ".*" ".*" ".*"`
4. 修改密码：`rabbitmqctl change_ password 用户名 新密码`
5. 删除用户：`rabbitmqctl delete_user 用户名`
6. 查看用户清单: `rabbitmqctl list_users`
### 角色
1. `administrator` :可以登录控制台、查看所有信息、并对rabbitmq进行管理
2. `monToring`:监控者；登录控制台，查看所有信息
3. `policymaker`:策略制定者；登录控制台指定策略
4. `managment`:普通管理员；登录控制

