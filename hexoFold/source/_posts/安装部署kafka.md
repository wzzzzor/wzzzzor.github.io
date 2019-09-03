---
title: 安装部署kafka
date: 2019-07-09 21:58:26
tags: [kafka]
---



## 环境准备

- **操作系统**：centos

- **kafka版本**：kafka_2.12-2.2.1      ----[下载地址](http://kafka.apache.org/downloads.html)

- **jdk**：jdk1.8.0_111

  

## 下载及压缩

- **下载**

  ```
  $ curl -L -O https://mirrors.cnnic.cn/apache/kafka/2.3.0/kafka_2.12-2.3.0.tgz
  ```

- **压缩**

  ```
  $ tar zxvf kafka_2.12-2.3.0.tgz
  ```



## **zookeeper**

- **下载zookeeper**

  ```
  $ curl -L -O http://apache.fayea.com/zookeeper/zookeeper-3.4.14/zookeeper-3.4.14.tar.gz
  ```

- **解压**

  ```
  $ tar zxvf zookeeper-3.4.14.tar.gz
  ```

- **设置环境变量**

  - **进入编辑**

  ```
  $ vim /etc/profile
  ```

  - **然后输入下述内容**

  ```
  JAVA_HOME=/opt/java/jdk1.8.0_111/
  ZOOKEEPER_HOME=/opt/zookeeper-3.4.14/
  PATH=$JAVA_HOME/bin:$ZOOKEEPER_HOME/bin:$PATH
  CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$ZOOKEEPER_HOME/lib:
  export ZOOKEEPER_HOME
  export JAVA_HOME
  export PATH
  export CLASSPATH
  
  ```

  - **保存环境变量配置**

  ```
  $ source /etc/profile
  ```

  

- **配置**

  配置文件放在$ZOOKEEPER_HOME/conf/目录下，将zoo_sample.cfd文件名称改为zoo.cfg

- **启动zookeeper**

  启动脚本在$ZOOKEEPER_HOME/bin/目录下

  ```
  $ ./bin/zkServer.sh start
  ```

  ```
  $ netstat -tunlp|grep 2181 #查看zookeeper端口
  ```



## 配置kafka

- **配置server.properties**

  ```
  broker.id=0
  num.network.threads=2
  num.io.threads=8
  socket.send.buffer.bytes=1048576
  socket.receive.buffer.bytes=1048576
  socket.request.max.bytes=104857600
  log.dirs=/tmp/kafka-logs
  num.partitions=2
  log.retention.hours=168
   
  log.segment.bytes=536870912
  log.retention.check.interval.ms=60000
  log.cleaner.enable=false
   
  zookeeper.connect=localhost:2181
  zookeeper.connection.timeout.ms=1000000
  ```

  [参考原博](https://blog.csdn.net/lizhitao/article/details/25667831)



## 启动kafka

- **启动命令**

  ```
  [root@localhost kafka_2.12-2.2.0]# ./bin/kafka-server-start.sh daemon ./config/server.properties &
  ```

  

- **检测端口**

  ```
  $ netstat -tunlp|egrep "(2181|9092)"
  ```

  