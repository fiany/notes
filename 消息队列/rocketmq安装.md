# rocketmq安装

## Linux安装

### 下载安装包

```bash
wget http://mirror.bit.edu.cn/apache/rocketmq/4.5.0/rocketmq-all-4.5.0-bin-release.zip
# 解压
unzip -d rockermq rocketmq-all-4.5.0-bin-release.zip
```

### 启动NameServer,默认端口为9876

```bash
sh ./bin/mqnamesrv
# 当前为前台启动
tail -f ~/logs/rocketmqlogs/namesrv.log
# 查看是否有启动成功日志
```

### 启动broker

```bash
sh ./bin/mqbroker -n 192.168.0.117:9876
tail -f ~/logs/rocketmqlogs/broker.log
# 查看是否有启动成功日志
The broker[%s, 192.168.0.117:10911] boot success...
```

#### sendDefaultImpl call timeout 错误

- 确保启动成功返回的日志中包含的ip客户端是可以访问的，否则会出现 **sendDefaultImpl call timeout**  错误,这种情况主要是服务器上面有多个网卡造成的， 启动的时候如果没有指定IP，会随机选中一个，导致外部无法放访问。

- 解决办法启动的时候指定ip

```bash
# 进入rocketmq根目录
# 指定IP
echo "brokerIP1=192.168.0.117" > broker.properties
# 在启动broker的时候指定配置文件
sh ./bin/mqbroker -n 192.168.0.117:9876 -c ./broker.properties
# 配置文件路径可能不一样，需要注意，再次查看启动日志，返回的ip如果是配置的信息那就没错了。
```

#### 启动之后默认占用内存比较大

- broker有一段配置指定启动占用的内存，我在测试环境测试的时候，本来测试服务器内存也不大只有8g,结果启动完直接内存占用完了，心想肯定是指定的内存过大了。
- 指定的配置在`runbroker.sh`中，有一段`JAVA_OPT="${JAVA_OPT} -server -Xms8g -Xmx8g -Xmn4g"`将其修改为`JAVA_OPT="${JAVA_OPT} -server -Xms128m -Xmx128m -Xmn128m"`, **注意：仅在测试环境这么配置，生产环境建议不要修改，不然生产环境只能用一段时间内存就满了**

### 常用命令

- 如果nameserver与broker都启动了需要停止
  - sh ./bin/mqshutdown broker
    sh ./bin/mqshutdown namesrv