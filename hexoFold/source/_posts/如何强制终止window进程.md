---
title: 如何强制终止window进程
date: 2019-07-21 09:55:02
tags:
---



## 强制终止8080端口

- 找到8080端口的进程id

```
CMD>netstat -ano | findstr 8080
```

- kil掉这个进程

```
CMD>taskkill /F /PID 1234
```

> 1234为查出来的进程id

