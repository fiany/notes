### 循环延时

```bash
for i in {1..3}
do
        echo "========重启中"$i"S"
        sleep 1s
done
```

### 远程连接服务器执行脚本

- 多条命令

```bash
# 将输出文件传入空位置   
# << eeooff开始   eeooff结束
ssh -t root@192.168.0.1 > /dev/null 2>&1 << eeooff
        cd /apps/test_console
        sh start.sh
        exit
eeooff
```

- 单条命令

```
ssh -t root@192.168.0.1 "cd /apps/test_console ; sh start.sh"
```

### 后台启动jar包

```bash
# 后台启动 ，并将文件输出至temp.log
nohup java -jar  app.jar --spring.profiles.active=test  > temp.log 2>&1 &
```

