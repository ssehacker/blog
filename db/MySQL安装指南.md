# MySQL 安装指南

按照[官网文档](https://www.linode.com/docs/databases/mysql/how-to-install-mysql-on-centos-7/)直接安装。

### 密码问题
安装之后默认密码记录在log中，可以这样查看：[如何查看密码](https://stackoverflow.com/questions/21944936/error-1045-28000-access-denied-for-user-rootlocalhost-using-password-y/42967789#42967789)

查看密码：
```
$ sudo grep 'temporary password' /var/log/mysqld.log
```

登陆mysql：
```
$ mysql -uroot -p 
```

