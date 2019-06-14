## RabbitMq安装

### centos安装

#### erlang安装

##### 下载地址

<https://packages.erlang-solutions.com/erlang/>

```bash
# 下载
wget https://packages.erlang-solutions.com/erlang/rpm/centos/7/x86_64/esl-erlang_22.0.1-1~centos~7_amd64.rpm
# 安装
yum install esl-erlang_22.0.1-1~centos~7_amd64.rpm
```

#### RabbitMq安装

##### 下载地址

<https://www.rabbitmq.com/install-rpm.html#downloads>

```bash
# 下载
wget https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.7.14/rabbitmq-server-3.7.14-1.el7.noarch.rpm
# 安装
yum install rabbitmq-server-3.7.14-1.el7.noarch.rpm
```

#### 执行报错

```bash
Error: Package: rabbitmq-server-3.7.14-1.el7.noarch (/rabbitmq-server-3.7.14-1.el7.noarch)
           Requires: erlang >= 20.3
           Available: erlang-R16B-03.18.el7.x86_64 (epel)
               erlang = R16B-03.18.el7
 You could try using --skip-broken to work around the problem
 You could try running: rpm -Va --nofiles --nodigest

```

版本问题选择跳过，使用下面的命令继续安装，安装成功。

```bash
rpm -ivh --nodeps rabbitmq-server-3.7.14-1.el7.noarch.rpm
```

### ubuntu安装rabbitmq

- 添加源

  将下面的源添加到`/etc/apt/sources.list`中。

  ```bash
  deb http://www.rabbitmq.com/debian/ testing main
  ```

- 更新

  ```bash
  sudo apt-get update
  ```

- 执行安装

  ```bash
  sudo apt-get install rabbitmq-server
  ```

  可能在安装中提示无法验证安装包问题，这是因为没有添加公钥到信任列表，可以这样：

  ```bash
  wget http://www.rabbitmq.com/rabbitmq-signing-key-public.asc
  ```

### 启动RabbitMq

```bash
# 启动RabbitMQ服务
service rabbitmq-server start
systemctl start rabbitmq-server
# 状态查看
rabbitmqctl status
# 启用插件
rabbitmq-plugins enable rabbitmq_management
# 重启服务
service rabbitmq-server restart
systemctl restart rabbitmq-server
# 添加帐号:name 密码:passwd
rabbitmqctl add_user name passwd
# 赋予其administrator角色
rabbitmqctl set_user_tags name administrator
# 设置权限
rabbitmqctl set_permissions -p / name ".*" ".*" ".*"
```

### guest无法远程访问

```bash
#将rabbitmq安装目录下ebin目录下rabbit.app中loopback_users里的guest删除，重启rabbitmq服务
find / -name rabbit.app
```

