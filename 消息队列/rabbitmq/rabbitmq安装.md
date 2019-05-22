## RabbitMq安装

### centos安装

#### erlang安装

##### 下载地址

<https://packages.erlang-solutions.com/erlang/>

```bash
# 下载
wget http://packages.erlang-solutions.com/site/esl/esl-erlang/FLAVOUR_1_general/esl-erlang_22.0-1~centos~7_amd64.rpm
# 安装
yum install esl-erlang_22.0-1~centos~7_amd64.rpm
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
# 状态查看
rabbitmqctl status
# 启用插件
rabbitmq-plugins enable rabbitmq_management
# 重启服务
service rabbitmq-server restart
# 添加帐号:name 密码:passwd
rabbitmqctl add_user name passwd
# 赋予其administrator角色
rabbitmqctl set_user_tags name administrator
# 设置权限
rabbitmqctl set_permissions -p / name ".*" ".*" ".*"
```

