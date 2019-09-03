---
layout: index
title: 如何将web项目部署到linux服务器
date: 2019-06-23 15:57:50
tags: [linux]
---

本文档意在让开发人员掌握如何将本地项目发包至远程服务器



## 下载远程工具(如:MobaXterm)
MobaXterm下载地址：https://mobaxterm.mobatek.net/download-home-edition.html

## 连接到开发测试服务器
- **服务器地址**：172.16.100.201
- **户名/密码**：root/root
>注意：这是开发服务器地址，如果想远程其他服务器，请自行更改地址以及账密

## <span id="jump">结束进程(停掉Tomcat)</span>
- **查询进程**：ps -ef|grep java
- **杀死进程**：kill -9 pid
	> 注意：pid是你要杀死的进程id

## 进入tomcat目录
两种方式到tomcat目录下：
- **方式一**：cd /opt/platform/tomcat-platform-portal/
- **方式二**：pt(自定义的快捷方式，我这里新建了一个shell脚本，脚本内容是配置pt为打开tomcat的快捷操作)

## 删除以及更新备份文件
- rm -rf portal-web/
- mv webapps/portal-web/ .
>注意：mv webapps/portal-web/ .命令中"."与"/"之间有个空格

## 传包到webapps目录下

## 启动Tomcat
bin/startup.sh

## 停掉Tomcat
>建议使用结束进程的方式停掉Tomcat,先查询该项目的进程id，再使用kill命令杀死进程，参考[结束进程](#jump)

## 删除war包
rm -f webapps/portal-web.war

## 再次启动Tomcat
bin/startup.sh 

## 查看日志
tail -f logs/catalina.out