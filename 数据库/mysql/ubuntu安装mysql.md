### msyql安装

- 安装服务端

```bash
apt-get install mysql-server-5.7
```

- 启动

```bash
service msyql start
```

- 修改配置文件

```bash
vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

> [mysqld_safe]
> socket          = /var/run/mysqld/mysqld.sock
> nice            = 0
>
> [mysqld]
> user            = mysql
> pid-file        = /var/run/mysqld/mysqld.pid
> socket          = /var/run/mysqld/mysqld.sock
> port            = 3307
> basedir         = /usr
> datadir         = /var/lib/mysql
> tmpdir          = /tmp
> lc-messages-dir = /usr/share/mysql
> skip-external-locking
>
> #bind-address           = 127.0.0.1

修改`bind-address = 127.0.0.1`使其他机器可以连接，`port`可以修改端口

- 查看数据库密码

```bash
vim /etc/mysql/debian.cnf
```

> [client]
> host     = localhost
> user     = debian-sys-maint
> password = 6QX2nnMR9Zcrgkua
> socket   = /var/run/mysqld/mysqld.sock
> [mysql_upgrade]
> host     = localhost
> user     = debian-sys-maint
> password = 6QX2nnMR9Zcrgkua
> socket   = /var/run/mysqld/mysqld.sock

- 登陆用户

```bash
mysql -u debian-sys-maint -P3307 -p
6QX2nnMR9Zcrgkua
```

- 修改账户密码

```mysql
update mysql.user set authentication_string=password('root'), host='%' where user='root';
```

- 重启数据库

```bash
service mysql restart
```

- 重新登陆

```bash
mysql -u root -P3307 -p
root
```

不出意外应该登陆成功

