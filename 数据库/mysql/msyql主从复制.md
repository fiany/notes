## mysql主从复制

为什么做主从复制，这里不做介绍，主要讲如何配置主从复制。
### 必要条件
主从复制至少我们需要启动两个数据库实例，保证两个数据库实例可以互相访问，这块我直接使用了最高权限的root用户（生产环境不要这样使用,数据库权限问题这里不做处理）。数据库实例可以互相访问意味着没有登陆ip的限制。

### 配置文件修改

假设此处已经知道数据库的配置文件所在位置。centos配置文件位置`/etc/my.cnf`

主数据库配置流程

- 开启二进制日志 binlog
- 配置唯一的server-id
- 获得master二进制文件名及位置

从数据库配置流程

- 配置唯一的server-id
- 使用master分配的用户账号读取master二进制日志
- 启动slave服务

#### 主数据库配置文件

```bash
[mysqld]
# 开启二进制日志
log-bin=mysql-bin 
# 设置server-id
server-id=110
```

#### 从数据库配置文件

```bash
#开启中继日志
relay-log=mysql-relay
#设置server-id
server-id=111
```

配置完成后重启数据库

```bash
systemctl restart mysqld
```

### 连接数据库

连接到主数据库执行命令

```bash
--查询master的状态
show master status\G
```

返回结果

```bash
mysql> show master status\G
*************************** 1. row ***************************
             File: mysql-bin.000001
         Position: 154
     Binlog_Do_DB: 
 Binlog_Ignore_DB: 
Executed_Gtid_Set: 
1 row in set (0.00 sec)
```

记录结果中File和Position的值。

连接到从数据库执行命令

```bash

CHANGE MASTER TO master_host = '172.16.0.6'
,master_user = 'root'
,master_password = 'root'
,master_log_file = 'mysql-bin.000001'
,master_log_pos = 154;
```

如果没报错继续执行命令

```bash
//开启复制
start slave;
//查看主从复制是否配置成功
SHOW SLAVE STATUS\G
```

返回结果

```bash
mysql> SHOW SLAVE STATUS\G
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 172.16.0.6
                  Master_User: root
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000001
          Read_Master_Log_Pos: 154
               Relay_Log_File: mysql-relay.000002
                Relay_Log_Pos: 320
        Relay_Master_Log_File: mysql-bin.000001
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB: 
          Replicate_Ignore_DB: 
           Replicate_Do_Table: 
       Replicate_Ignore_Table: 
      Replicate_Wild_Do_Table: 
  Replicate_Wild_Ignore_Table: 
                   Last_Errno: 0
                   Last_Error: 
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 154
              Relay_Log_Space: 523
              Until_Condition: None
               Until_Log_File: 
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File: 
           Master_SSL_CA_Path: 
              Master_SSL_Cert: 
            Master_SSL_Cipher: 
               Master_SSL_Key: 
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error: 
               Last_SQL_Errno: 0
               Last_SQL_Error: 
  Replicate_Ignore_Server_Ids: 
             Master_Server_Id: 140
(下面还有并未复制完全，不过并不重要)
```

当看到Slave_IO_State:Waiting for master ot send event 、Slave_IO_Running: YES、Slave_SQL_Running: YES才表明状态正常。

**到此处配置完成**

### 结果测试

具体如何测试简单描述下，在主库创建一个数据库，再建张表，插入一条数据，如果在从库可以正常查询，则此处配置完成。

