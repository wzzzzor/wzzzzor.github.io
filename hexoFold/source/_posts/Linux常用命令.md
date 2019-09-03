---
title: Linux常用命令
date: 2019-06-16 18:25:46
tags: [linux]
catagories: 搭建博客
---



## linux常用命令

- **解压tar包到java目录下**：

```
# tar zxvf /opt/jdk-8u111-linux-x64.tar.gz -C /opt/java
```

- **重名名文件**：

```
# mv 原文件名 目标名
```

- **删除文件**：

```
# rm -f 文件路径
```

- **删除文件夹及子文件夹** ：

```
# rm -rf 文件夹路径
```

- **重启服务器**：

```
# reboot
```

- **查看是否安装dos2unix**：

```
# yum list dos2unix
# yum list（查看所有）
```

- **给文件加可执行权限**：

```
# chmod +x filename
```

- **给文件加最高权限**：

```
# chmod 777 filename
```

- **查看centos版本**：

```
# cat /etc/redhat-release
# rpm -q centos-release
```

- **查看nginx版本信息**：

```
# /opt/nginx/sbin/nginx -v
```

- **查看端口占用情况**：

```
# netstat -anp|grep port
```

- **检查内存情况**：

```
# grep MemTotal /proc/meminfo
# grep SwapTotal /proc/meminfo

```

- **字符串搜索**：

```
# grep 目标字符串 -rl .
```

- **字符串批量替换**:

```
# sed -i "s/old/new/g" `grep old -rl .`
#sed -i "s/192.168.100.165/192.168.102.57/g" `grep 192.168.100.165 -rl .`
```

- **nohup 运行jar包文件程序**

```
 nohup java -jar XXX.jar >nohup.out &
```

- **将文件的最后20行输出到设备**：

  ```
  tail -n 20 log.txt
  ```

  



## firewall 命令：

- **查看已经开放的端口**：

```
# firewall-cmd --list-ports
```

- **开启8800端口**：

```
# firewall-cmd --zone=public --add-port=8800/tcp --permanent
```

- **禁用8800端口**：

```
# firewall-cmd --zone=public --remove-port=8800/tcp --permanent
```

- **重启firewall**：
  
```
# firewall-cmd --reload
```

- **停止firewall**：
  
```
# systemctl stop firewalld.service
```

- **禁止firewall开机启动**：
  
```
# systemctl disable firewalld.service
```

- **查看防火墙是否开启**：

```
# systemctl status firewalld
```

> **–zone**： 作用域

> **–add-port=80/tcp**：添加端口，格式为：端口/通讯协议

> **–permanent**：永久生效，没有此参数重启后失效

