## docker安装

- ubuntu 

  执行两个命令：

  sudo wget -qO- https://get.docker.com | sh

  sudo usermod -aG docker fiany 将该用户加入到docker组中



- 进入容器内部
  - docker exec -it [容器id]  /bin/bash

