linux安装redis

```bash
# ubuntu
apt-get install redis-server
# centos
yum install redis
```

linux启动redis

```bash
# 前台启动
redis-server
# 后台启动
redis-server & 
```

连接redis

```bash
redis-cli -c -h 10.3.34.101 -p 7000
```

linux关闭redis

- 连上服务器后执行shutdown命令



