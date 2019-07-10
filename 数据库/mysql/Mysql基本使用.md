## mysql初次使用

##### 远程连接使用utf-8编码

- mysql -u root -p --default-character-set=utf8

##### 创建数据库默认utf-8

- CREATE DATABASE IF NOT EXISTS my_db default charset utf8 COLLATE utf8_general_ci; 

##### 创建可以远程登陆的用户

- `CREATE USER 'root'@'%' IDENTIFIED BY 'root';`

##### 用户密码30天过期

- CREATE USER IF NOT EXISTS 'test' IDENTIFIED BY 'test' PASSWORD EXPIRE INTERVAL 30 DAY;

##### 用户密码永不过期

- CREATE USER IF NOT EXISTS 'test' IDENTIFIED BY 'test' PASSWORD EXPIRE NEVER;

##### 数据库密码不能设置过于简单

```bash
# 设置密码安全程度为最低
set global validate_password_policy=0;
# 设置密码长度
set global validate_password_length=4;
```

##### 登陆状态修改用户密码

- update mysql.user set authentication_string=password('root') where user='root' ; 

##### 授权testdb库的所有权限给用户test

- GRANT ALL ON testdb.* TO 'test';

##### 修改用户可以远程登陆

```bash
use mysql; 
update user set host = '%' where user = 'root';  
```

##### 刷新系统权限表

- flush privileges;

## docker中的MySQL操作

- 将数据拷入docker容器中
  - docker cp human_all.sql 65dabd57029b:/root/
- 导入数据
  - mysql -uroot -p human_society < human_all.sql
- 进入容器
  - docker exec -it c3bd32e48b21 bash