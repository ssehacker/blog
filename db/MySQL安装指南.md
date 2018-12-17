# MySQL 安装指南

按照[官网文档](https://www.linode.com/docs/databases/mysql/how-to-install-mysql-on-centos-7/)直接安装。

### 密码问题
安装之后默认密码记录在log中，可以这样查看：[如何查看密码](https://stackoverflow.com/questions/21944936/error-1045-28000-access-denied-for-user-rootlocalhost-using-password-y/42967789#42967789)

### 查看密码：
```
$ sudo grep 'temporary password' /var/log/mysqld.log
```

### 登陆mysql：
```
$ mysql -uroot -p 
```

### 修改mysql密码：
```
SET PASSWORD = PASSWORD('your_new_password');
```
密码需要设置的复杂一些，如：`Your_pwd/1`

### 创建用户及数据库
同样按照[官网文档](https://www.linode.com/docs/databases/mysql/how-to-install-mysql-on-centos-7/)操作。


#### 创建用户
```
CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';
```

```
flush privileges; # 这是mysql的bug，需要强行flush，否则create User不会生效。
```


### utf8编码问题
```shell
Incorrect string value: '\xE7\xA8\x8B\xE5\xBA\x8F...' for column 'course' at row 1
```

如果遇到上述错误，那是因为mysql默认不是utf8编码，而且mysql中的utf8不是真正的utf8，对于版本`v5.5.3+`的用户可以按如下操作：

1. 编辑mysql配置文件`/etc/my.cnf`文件(也可能是`/etc/my.cnf /etc/mysql/my.cnf /usr/etc/my.cnf ~/.my.cnf` 其中之一)，并添加以下信息：

```
[client]
default-character-set = utf8mb4

[mysql]
default-character-set = utf8mb4

[mysqld]
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
```

2. 对于已经存在的数据库or表，则还需要手动修改：

```
# For each database:
ALTER DATABASE database_name CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;
# For each table:
ALTER TABLE table_name CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
# For each column:
ALTER TABLE table_name CHANGE column_name column_name VARCHAR(191) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
# (Don’t blindly copy-paste this! The exact statement depends on the column type, maximum length, and other properties. The above line is just an example for a `VARCHAR` column.)
```

详细可参考[这篇文章](https://mathiasbynens.be/notes/mysql-utf8mb4)

3. 重启mysql：
```
/etc/init.d/mysqld restart
```

4. 如果使用的是`sequelize`数据持久层框架，则设计数据库的时候需要额外传`charset`参数：
```
sequelize.define('demo_table', {
  id: {
    type: Sequelize.INTEGER,
    autoIncrement: true,
    allowNull: false,
    primaryKey: true,
  },

  // 名称
  name: {
    type: Sequelize.STRING,
  },

  description: {
    type: Sequelize.STRING,
  },
}, { charset: 'utf8mb4' });
```


