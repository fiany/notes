linux安装redis

```bash
apt-get install redis-server
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



