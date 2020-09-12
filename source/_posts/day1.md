---
title: 访问网页出错
date: 2020-09-11 16:48:02
tags: HSTS
categories: 错误积累
description: 解决:您目前无法访问 因为此网站使用了 HSTS。网络错误和攻击通常是暂时的，因此，此网页稍后可能会恢复正常
---



1. 在Chrome地址栏中输入:   chrome://net-internals/#hsts; 进入Domain Sercurity Policy界面。
2. 在下图中输入二级域名查询是否使用了强制 HTTPS 请求。
![错误](cuowu.png)
3. 如果有查询结果，则在最下方的delete栏处，删除该域名的信息
![删除](delete.png)
4. 再次查询，如下图所示，“NOT FOUND”则表示删除成功。
![重新查询](restore.png)

