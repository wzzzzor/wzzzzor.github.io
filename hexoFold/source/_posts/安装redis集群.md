---
title: 安装redis集群
date: 2019-07-09 21:58:26
tags: [redis]
---



## 上传压缩包

将redis-5.0.4.tar.gz压缩包上传到opt目录下



## 解压

```
[root@localhost ~]# cd /opt/
[root@localhost opt]# tar -zxvf redis-5.0.4
```



## 安装gcc gcc-c++

```
[root@localhost opt]# yum install gcc gcc‐c++ libstdc++‐devel
```



## 编译安装

```
[root@localhost opt]# cd ./redis-5.0.4
[root@localhost redis-5.0.4]# make
[root@localhost redis-5.0.4]# make install
```



## redis-trib.rb

将 redis-trib.rb 复制到 /usr/local/bin 目录下

```
[root@localhost redis-5.0.4]# cd src/
[root@localhost src]# cp redis-trib.rb /usr/local/bin/
```



## 创建redis节点

- **创建redis_cluster目录**

  ```
  [root@localhost src]# cd ~
  [root@localhost ~]# mkdir /redis_cluster
  ```

- **在redis_cluster目录下，创建7000、7001、7002、7003、7004、7005的目录，并将redis.conf拷贝到这六个目录**

  ```
  [root@localhost ~]# cd /redis-cluster/
  [root@localhost redis-cluster]# mkdir 7000
  [root@localhost redis-cluster]# mkdir 7001
  [root@localhost redis-cluster]# mkdir 7002
  [root@localhost redis-cluster]# mkdir 7003
  [root@localhost redis-cluster]# mkdir 7004
  [root@localhost redis-cluster]# mkdir 7005
  [root@localhost redis-cluster]# cp /opt/redis-5.0.4/redis.conf ./7000/
  [root@localhost redis-cluster]# cp /opt/redis-5.0.4/redis.conf ./7001/
  [root@localhost redis-cluster]# cp /opt/redis-5.0.4/redis.conf ./7002/
  [root@localhost redis-cluster]# cp /opt/redis-5.0.4/redis.conf ./7003/
  [root@localhost redis-cluster]# cp /opt/redis-5.0.4/redis.conf ./7004/
  [root@localhost redis-cluster]# cp /opt/redis-5.0.4/redis.conf ./7005/
  ```

- **分别修改这六个配置文件**

  ```
  port  7000                                        
  //端口7000,7001,7002,7003,7004,7005        
  
  bind 本机ip                                       
  //默认ip为127.0.0.1 需要改为其他节点机器可访问的ip 否则创建集群时无法访问对应的端口，无法创建集群
  
  daemonize    yes                               
  //redis后台运行
  
  pidfile  /var/run/redis_7000.pid          
  //pidfile文件对应7000,7001,7002,7003,7004,7005
  
  cluster-enabled  yes                           
  //开启集群  把注释#去掉
  
  cluster-config-file  nodes_7000.conf   
  //集群的配置  配置文件首次启动自动生成 7000,7001,7002
  
  cluster-node-timeout  15000                
  //请求超时  默认15秒，可自行设置
  
  appendonly  yes                           
  //aof日志开启  有需要就开启，它会每次写操作都记录一条日志
  ```



## 启动各节点

```
[root@localhost redis-cluster]# cd /usr/local/bin/
[root@localhost bin]# redis-server /redis-cluster/7000/redis.conf
[root@localhost bin]# redis-server /redis-cluster/7001/redis.conf
[root@localhost bin]# redis-server /redis-cluster/7002/redis.conf
[root@localhost bin]# redis-server /redis-cluster/7003/redis.conf
[root@localhost bin]# redis-server /redis-cluster/7004/redis.conf
[root@localhost bin]# redis-server /redis-cluster/7005/redis.conf
```

每个节点启动成功之后显示如下信息：

```
2002:C 09 Jul 2019 10:09:37.373 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
2002:C 09 Jul 2019 10:09:37.374 # Redis version=5.0.4, bits=64, commit=00000000, modified=0, pid=2002, just started
2002:C 09 Jul 2019 10:09:37.374 # Configuration loaded
```



## 检查redis启动情况

```
[root@localhost bin]# ps -ef|grep redis
root      2003     1  0 10:09 ?        00:00:03 redis-server 0.0.0.0:7000 [cluster]
root      2008     1  0 10:09 ?        00:00:03 redis-server 0.0.0.0:7001 [cluster]
root      2014     1  0 10:09 ?        00:00:03 redis-server 0.0.0.0:7002 [cluster]
root     19567     1  0 10:37 ?        00:00:01 redis-server 0.0.0.0:7003 [cluster]
root     19575     1  0 10:37 ?        00:00:01 redis-server 0.0.0.0:7004 [cluster]
root     19582     1  0 10:37 ?        00:00:01 redis-server 0.0.0.0:7005 [cluster]
root     20314  1946  0 10:52 pts/1    00:00:00 grep --color=auto redis

[root@localhost bin]# netstat -tnlp|grep redis
tcp        0      0 0.0.0.0:7004            0.0.0.0:*               LISTEN      19575/redis-server
tcp        0      0 0.0.0.0:7005            0.0.0.0:*               LISTEN      19582/redis-server
tcp        0      0 0.0.0.0:17000           0.0.0.0:*               LISTEN      2003/redis-server 0
tcp        0      0 0.0.0.0:17001           0.0.0.0:*               LISTEN      2008/redis-server 0
tcp        0      0 0.0.0.0:17002           0.0.0.0:*               LISTEN      2014/redis-server 0
tcp        0      0 0.0.0.0:17003           0.0.0.0:*               LISTEN      19567/redis-server
tcp        0      0 0.0.0.0:17004           0.0.0.0:*               LISTEN      19575/redis-server
tcp        0      0 0.0.0.0:17005           0.0.0.0:*               LISTEN      19582/redis-server
tcp        0      0 0.0.0.0:7000            0.0.0.0:*               LISTEN      2003/redis-server 0
tcp        0      0 0.0.0.0:7001            0.0.0.0:*               LISTEN      2008/redis-server 0
tcp        0      0 0.0.0.0:7002            0.0.0.0:*               LISTEN      2014/redis-server 0
tcp        0      0 0.0.0.0:7003            0.0.0.0:*               LISTEN      19567/redis-server
```


## 安装ruby

```
[root@localhost bin]# yum -y install ruby ruby-devel rubygems rpm-build
[root@localhost bin]# gem install redis
```

> 注意：当运行gem install redis出现以下类似信息，说明ruby版本过低,需要手动升级版本
>
> ```
> Fetching: redis-4.1.2.gem (100%)
> ERROR:  Error installing redis:
>         redis requires Ruby version >= 2.3.0
> ```



## 升级ruby（如果ruby版本过低，执行此步骤）

  - **安装RVM**

    ```
    [root@localhost bin]# gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
    [root@localhost bin]# curl -L get.rvm.io | bash -s stable
    [root@localhost bin]# find / -name rvm -print
    /usr/local/rvm
    /usr/local/rvm/src/rvm
    /usr/local/rvm/src/rvm/bin/rvm
    /usr/local/rvm/src/rvm/lib/rvm
    /usr/local/rvm/src/rvm/scripts/rvm
    /usr/local/rvm/bin/rvm
    /usr/local/rvm/lib/rvm
    /usr/local/rvm/scripts/rvm
    ```

    

  - ```
    [root@localhost bin]# source /usr/local/rvm/scripts/rvm
    ```

  - **查看rvm库中已知的ruby版本**

    ```
    [root@localhost bin]# rvm list known
    ```

    

  - **安装一个ruby版本**

    ```
    [root@localhost bin]# rvm install 2.3.3
    ```

    

  - **使用一个ruby版本**

    ```
    [root@localhost bin]# rvm use 2.3.3
    ```

    

  - **设置默认版本**

    ```
    [root@localhost bin]# rvm use 2.3.3 --default
    ```

    

  - **卸载一个已知版本**

    ```
    rvm remove 2.0.0
    ```

  

  - **再次安装redis**

  ```
  [root@localhost bin]# gem install redis
  Fetching redis-4.1.2.gem
  Successfully installed redis-4.1.2
  Parsing documentation for redis-4.1.2
  Installing ri documentation for redis-4.1.2
  Done installing documentation for redis after 2 seconds
  1 gem installed
  ```



## 创建集群

  ```
  [root@localhost bin]# redis-cli --cluster create 192.168.1.138:7000 192.168.1.138:7001 192.168.1.138:7002 192.168.1.138:7003 192.168.1.138:7004 192.168.1.138:7005 --cluster-replicas 1
  >>> Performing hash slots allocation on 6 nodes...
  Master[0] -> Slots 0 - 5460
  Master[1] -> Slots 5461 - 10922
  Master[2] -> Slots 10923 - 16383
  Adding replica 192.168.1.138:7004 to 192.168.1.138:7000
  Adding replica 192.168.1.138:7005 to 192.168.1.138:7001
  Adding replica 192.168.1.138:7003 to 192.168.1.138:7002
  >>> Trying to optimize slaves allocation for anti-affinity
  [WARNING] Some slaves are in the same host as their master
  M: 7da3ac110691ea9a7c188eac14067512285d2de2 192.168.1.138:7000
     slots:[0-5460] (5461 slots) master
  M: d3285c16dd1d7e4557976349cc47de6c09ffabd4 192.168.1.138:7001
     slots:[5461-10922] (5462 slots) master
  M: 4ba9babdfb31976e74574e79e200b91d995ccc21 192.168.1.138:7002
     slots:[10923-16383] (5461 slots) master
  S: 0015185c60ef3c029a323b1fd7ab11f4ffacd66e 192.168.1.138:7003
     replicates d3285c16dd1d7e4557976349cc47de6c09ffabd4
  S: 8b2e59376a9d90359b123d28fc978448d01233a3 192.168.1.138:7004
     replicates 4ba9babdfb31976e74574e79e200b91d995ccc21
  S: 47f96a39880e465e046bf9e624f4d5cdcb824ede 192.168.1.138:7005
     replicates 7da3ac110691ea9a7c188eac14067512285d2de2
  Can I set the above configuration? (type 'yes' to accept):
  ```

  输入 yes 即可，然后出现如下内容，说明安装成功。

  ```
  >>> Nodes configuration updated
  >>> Assign a different config epoch to each node
  >>> Sending CLUSTER MEET messages to join the cluster
  Waiting for the cluster to join
  .
  >>> Performing Cluster Check (using node 192.168.1.138:7000)
  M: 7da3ac110691ea9a7c188eac14067512285d2de2 192.168.1.138:7000
     slots:[0-5460] (5461 slots) master
     1 additional replica(s)
  M: d3285c16dd1d7e4557976349cc47de6c09ffabd4 192.168.1.138:7001
     slots:[5461-10922] (5462 slots) master
     1 additional replica(s)
  S: 0015185c60ef3c029a323b1fd7ab11f4ffacd66e 192.168.1.138:7003
     slots: (0 slots) slave
     replicates d3285c16dd1d7e4557976349cc47de6c09ffabd4
  M: 4ba9babdfb31976e74574e79e200b91d995ccc21 192.168.1.138:7002
     slots:[10923-16383] (5461 slots) master
     1 additional replica(s)
  S: 47f96a39880e465e046bf9e624f4d5cdcb824ede 192.168.1.138:7005
     slots: (0 slots) slave
     replicates 7da3ac110691ea9a7c188eac14067512285d2de2
  S: 8b2e59376a9d90359b123d28fc978448d01233a3 192.168.1.138:7004
     slots: (0 slots) slave
     replicates 4ba9babdfb31976e74574e79e200b91d995ccc21
  [OK] All nodes agree about slots configuration.
  >>> Check for open slots...
  >>> Check slots coverage...
  [OK] All 16384 slots covered.
  
  ```



## 开启端口

```
[root@localhost bin]# firewall-cmd --zone=public --add-port=7000/tcp --permanent
success
[root@localhost bin]# firewall-cmd --zone=public --add-port=7001/tcp --permanent
success
[root@localhost bin]# firewall-cmd --zone=public --add-port=7002/tcp --permanent
success
[root@localhost bin]# firewall-cmd --zone=public --add-port=7003/tcp --permanent
success
[root@localhost bin]# firewall-cmd --zone=public --add-port=7004/tcp --permanent
success
[root@localhost bin]# firewall-cmd --zone=public --add-port=7005/tcp --permanent
success
[root@localhost bin]# firewall-cmd --zone=public --add-port=17005/tcp --permanent
success
[root@localhost bin]# firewall-cmd --zone=public --add-port=17004/tcp --permanent
success
[root@localhost bin]# firewall-cmd --zone=public --add-port=17003/tcp --permanent
success
[root@localhost bin]# firewall-cmd --zone=public --add-port=17002/tcp --permanent
success
[root@localhost bin]# firewall-cmd --zone=public --add-port=17001/tcp --permanent
success
[root@localhost bin]# firewall-cmd --zone=public --add-port=17000/tcp --permanent
success
```

开启之后重启防火墙

```
[root@localhost bin]# firewall-cmd --reload
```


























