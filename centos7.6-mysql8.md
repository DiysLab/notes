# centos7.6安装mysql8

## 安装mysql
1. 删除mariadb
```shell
rpm -qa | grep mariadb
yum remove mariadb-libs -y
```
2. 下载rmp文件
```shell
wget https://dev.mysql.com/get/mysql80-community-release-el7-7.noarch.rpm
```
3. 安装rmp
```shell
rpm -ivh mysql80-community-release-el7-7.noarch.rpm
```
4. 安装mysql
```shell
yum install mysql-server -y
```
5. 启动mysql-server
```shell
service mysqld start
```
6. 查看是否启动成功
```shell
ps -ef |grep mysqld
netstat -anp | grep 3306
```
7. 添加卡开机启动
```shell
chkconfig mysqld on
```

## 修改端口号

1. 修改配置文件
```shell
vim /etc/my.cnf
在[mysqld]里 添加 "port=6666"
```
2. 重启mysql-server
```shell
service mysqld restart
```

## 无密码登录

1. 修改配置文件
```shell
vim /etc/my.cnf
在[mysqld]里 添加 "skip-grant-tables"
```
2. 重启mysql-server
```shell
service mysqld restart
```

## 设置密码为空字符串

1. 登录
```shell
mysql -u root
```
2. 执行sql命令
```shell
mysql > use mysql;
mysql > update user set authentication_string='' where user='root';
exit;
```
3. 注释"skip-grant-tables"
```shell
vim /etc/my.cnf
#skip-grant-tables
```
4. 重启mysql-server
```shell
service mysqld restart
```

## 设置密码

1. 登录
```shell
mysql -u root -p
Enter password: 直接换行
```
2. 设置密码
```shell
mysql > ALTER USER 'root'@'localhost' IDENTIFIED BY '你的密码';
mysql > exit;
```

## 设置root用户远程登录

1. 登录
```shell
mysql -u root -p
Enter password: 你的密码
```
2. 使用'mysql'数据库
```shell
mysql > use mysql;
```
3. 特定用户的host修改
```shell
mysql > update user set host='%' where user='root';
```
4. 允许root用户远程登录
```shell
Grant all privileges on root.* to 'root'@'%';
```
5. 设置密码
```shell
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '你的密码';
```
6. 刷新权限
```shell
flush privileges;
exit;
```
