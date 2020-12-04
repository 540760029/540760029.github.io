---
title: nginx
description: 正文被隐藏
date: 2020-11-30 12:50:52
tags: nginx
categories: 服务器
copyright: true
---

> Linux版本： CentOS7 64位
## nginx 安装

**在安装nginx前首先要确认系统中安装了gcc、pcre-devel、zlib-devel、openssl-devel。**安装的命令如下：
```bash
yum -y install gcc pcre-devel zlib-devel openssl openssl-devel
```
nginx 下载地址：<http://nginx.org/download>
下载：“nginx-1.9.9.tar.gz” 移动到/usr/local/ 下。

![](https://pic.downk.cc/item/5fc47bd7d590d4788a1b5f6b.jpg)

```bash
## 解压
tar -zxvf nginx-1.9.9.tar.gz

##进入nginx 目录：
cd nginx-1.9.9

##配置
./configure --prefix=/usr/local/nginx

## make
make
make install
```
> 注解：源码的安装一般有3个步骤组成：配置(configure)、编译(make)、安装(makeinstall)。 `Configure`是一个可执行脚本，它有很多选项，在待安装的源码路径下使用命令`./configure -–help`输出详细的选项列表。其中`--prefix`选项是配置安装的路径，如果不配置该选项，安装后可执行文件默认放在`/usr/local/bin`，库文件默认放在`/usr/local/lib`，配置文件默认放在`/usr/local/etc`，其它的资源文件放在`/usr/local/share`，比较凌乱。如果配置--prefix，如：`./configure --prefix=/usr/local/nginx`
可以把所有资源文件放在`/usr/local/nginx`的路径中，不会杂乱。
用了—prefix选项的另一个好处是卸载软件或移植软件。当某个安装的软件不再需要时，只须简单的删除该安装目录，就可以把软件卸载得干干净净；移植软件只需拷贝整个目录到另外一个机器即可（相同的操作系统）。
![](https://pic.downk.cc/item/5fc47e54d590d4788a1c8ebf.jpg)
![](https://pic.downk.cc/item/5fc47fd8d590d4788a1d5f9a.jpg)


### 测试是否安装成功：
```bash
## cd 到安装目录 /usr/local/nginx
./sbin/nginx -t
```
安装成功的显示：
![](https://pic.downk.cc/item/5fc48080d590d4788a1dac6b.jpg)

### 启动nginx:

```bash
cd /usr/local/nginx/sbin
./nginx
```
在浏览器输入ip 地址就可以看到结果了

### 修改默认访问地址：
![](https://pic.downk.cc/item/5fc485c8d590d4788a1fd7ae.jpg)
![](https://pic.downk.cc/item/5fc4864bd590d4788a200ff2.jpg)
![](https://pic.downk.cc/item/5fc48700d590d4788a205b06.jpg)

根据访问路径在相应的位置创建好的文件夹:放入测试文件
![](https://pic.downk.cc/item/5fc48741d590d4788a206e89.jpg)

### 重启nginx 服务：
```bash
## 进入到 /usr/local/nginx/sbin
./nginx -s reload
```
## 常用命令

```bash

#查看nginx 的版本：
./nginx -v

#关闭nginx：
./nginx -s stop

# 启动nginx:
./nginx

#查看nginx的进程：
ps -ef | grep nginx

#重启nginx:
./nginx -s reload
```
## 配置文件
> /usr/local/nginx/conf/nginx.conf
### 组成部分
#### 全局块
从配置文件开始到events块之间的内容，主要会设置一些影响nginx服务器整体运行的配置指令。例如：
`worker_processes 1` 支持的并发处理量
#### events块
影响Nginx服务器与用户的网络链接，例如：
`worker_connections 1024` 支持最大链接数
#### http块

### 配置实例：
1. 反向代理

` prooxy_pass http://127.0.0.1:8080` 
访问ip 跳转到8080端口

- 根据路径跳转到不同的端口 

再写一个server
![](https://pic.downk.cc/item/5fc8afea394ac5237847dd77.jpg)

![](https://pic.downk.cc/item/5fc8b156394ac5237849641b.jpg)

2. 负载均衡

![](https://pic.downk.cc/item/5fc8b37a394ac523784a49d0.jpg) 

**分配策略**
- 轮询策略 ：每个请求安时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除。默认策略
- weight : 权重，默认为1，权重越高被分配的客户端越多
- ip_hash : 每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器
- fair（第三方）：按后端服务器的响应时间来分配请求，响应时间短的服务优先分配

### 动静分离
> 动静分离：动态请求 和 静态请求 的两种请求。 两种方式 分开发布， 合并发布，用nginx分开

通过location 指定不同的后缀名实现不同的请求转发。通过expires 参数设置，可以
![](https://pic.downk.cc/item/5fc8ccf9394ac52378583cda.jpg)
![](https://pic.downk.cc/item/5fc8ce73394ac52378594734.jpg)

`autoindex on;` 列出文件夹下的列表

### 高可用
![](https://pic.downk.cc/item/5fc8d0ad394ac523785b381e.jpg)

需要两台ngnix、keepalived、 虚拟ip.

```bash
## 安装keepalived
yum install keepalived -y

##启动keeplived
systemctl start keepalived.service

##停止keeplived 
systemctl stop keeplived.service

##安装目录配置文件在：/etc/keepalived/keepalived.conf
```
- 配置高可用


```bash
## 网卡查看：
ifconfig

## 查看主机与配置
vi /etc/hosts
## 添加：127.0.0.1  LVS_DEVEL   （随便写）
```

keepalived.conf
```bash
## 全局配置
global_defs {
	notification_email {
		acassen@firewall.loc
		failover@firewall.loc
		sysadmin@firewall.loc
	}
	notification_email_from Alexandre.Cassen@firewall.loc
	smtp_server 192.168.17.129
	smtp_connect_timeout 30
	router_id LVS_DEVEL  ## 访问的主机
}
## 脚本配置
vrrp_script chk_http_port {
	script "/usr/local/src/nginx_check.sh"
	interval 2 #（检测脚本执行的间隔）
	weight 2 # 权重 
}

##虚拟ip 配置
vrrp_instance VI_1 {
	state BACKUP # 备份服务器上将 MASTER 改为 BACKUP
	interface ens33 # 网卡
	virtual_router_id 51 # 主、备机的 virtual_router_id 必须相同
	priority 90 # 主、备机取不同的优先级，主机值较大，备份机值较小
	advert_int 1 # 每隔1秒发送一个心跳
	authentication {
		auth_type PASS
		auth_pass 1111
	}
	virtual_ipaddress {
		192.168.17.50 # VRRP H 虚拟地址
	}
}
```


 /usr/local/src 添加检测脚本：nginx_check.sh
 ```bash
 #!/bin/bash
A=`ps -C nginx – no-header |wc -l`
if [ $A -eq 0 ];then
	/usr/local/nginx/sbin/nginx
	sleep 2
	if [ `ps -C nginx --no-header |wc -l` -eq 0 ];then
		killall keepalived
	fi
fi

 ```

## nginx 工作原理

```bash
## 查看nginx 进程
ps -ef |grep nginx
```
![](https://pic.downk.cc/item/5fc8dca2394ac523786533b8.jpg)

![](https://pic.downk.cc/item/5fc8dc74394ac5237865226f.jpg)
![](https://pic.downk.cc/item/5fc8dce7394ac52378654864.jpg)

- 首先，对于每个 worker 进程来说，独立的进程，不需要加锁，所以省掉了锁带来的开销，同时在编程以及问题查找时，也会方便很多。

- 其次，采用独立的进程，可以让互相之间不会影响，一个进程退出后，其它进程还在工作，服务不会中断， master 进程则很快启动新的worker进程。

- 当然， worker 进程的异常退出，肯定是程序有 bug 了，异常退出，会导致当前 worker 上的所有请求失败，不过不会影响到所有请求，所以降低了风险


> 参考：<https://www.cnblogs.com/xxoome/p/5866475.html>