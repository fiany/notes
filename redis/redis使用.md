## linux安装redis

```bash
# ubuntu
apt-get install redis-server
# centos
yum install redis
```

## linux启动redis

```bash
# ubuntu启动
# 前台启动
redis-server
# 后台启动
redis-server & 
# centos 启动
redis-server /etc/redis.conf
```

## 配置文件修改

 /etc/redis.conf

### 配置外网访问

```bash
# 注释只能通过本机访问
# bind 127.0.0.1 
# 关闭保护模式
protected-mode no
```

## 连接redis

```bash
redis-cli -c -h 10.3.34.101 -p 7000
```

linux关闭redis

- 连上服务器后执行shutdown命令
- redis-cli shutdown



