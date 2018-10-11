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



