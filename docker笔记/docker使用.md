## docker安装

- ubuntu 

  执行两个命令：

  sudo wget -qO- https://get.docker.com | sh

  sudo usermod -aG docker fiany 将该用户加入到docker组中

- 进入容器内部

  - docker exec -it [容器id]  /bin/bash

### 容器时间与当前时间相差八小时

- 还未打包镜像的情况,Dockerfile添加命令

```bash
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo 'Asia/Shanghai' >/etc/timezone
```

- 镜像已经在运行

```bash
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
echo 'Asia/Shanghai' >/etc/timezone
```

### 启动mysql

- windows

```bash
docker run --name some-mysql -v F:/dockerData/mysql/data:/var/lib/mysql  -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 -d mysql:5.7.23
```

- linux

```bash
docker run --name some-mysql -v /data/mysql/data:/var/lib/mysql -v /data/mysql/my.cnf:/etc/mysql/mysql.conf.d/mysqld.cnf -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 -d mysql:5.7.23
```

### 启动redis

- windows

```bash
docker run --name some-redis -v F:/dockerData/redis/data:/data -p 6379:6379 -d redis
```

### 启动rebbitmq

- windows

```bash
docker run -d --name some-rabbit  -p 15672:15672 rabbitmq:3-management
```

