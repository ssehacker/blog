# Postgres安装及简单使用指南


### ubuntu下如何安装？
参考资料：[https://www.godaddy.com/garage/how-to-install-postgresql-on-ubuntu-14-04/](https://www.godaddy.com/garage/how-to-install-postgresql-on-ubuntu-14-04/)


### 开放远程端口
默认postgres是绑定localhost的，要远程连接则按如下操作：
[https://blog.bigbinary.com/2016/01/23/configure-postgresql-to-allow-remote-connection.html](https://blog.bigbinary.com/2016/01/23/configure-postgresql-to-allow-remote-connection.html)



### 处理问题中几个常用的linux命令
1. `netstat -nlt`
查看进程及端口、绑定的host

2. 查看远程服务器端口是否开放
telnet 118.24.153.35 5432



### trouble shooting

1. 如果遇到如下问题：
```sh
psql: could not connect to server: No such file or directory
    Is the server running locally and accepting
    connections on Unix domain socket "/var/run/postgresql/.s.PGSQL.5432"?
```
解决方案： https://askubuntu.com/questions/50621/cannot-connect-to-postgresql-on-port-5432


