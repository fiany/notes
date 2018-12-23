## gitlab使用

### 为什么使用gitlab

### gitlab相对其他服务的有点

### gitlab本地部署

#### 直接安装

#### docker安装

```bash
# 配置hostname 在gitlab页面复制地址会使用该地址，如果不设置放的复制的地址会是 http://localhost
# 启动需要几分钟，未启动完毕，会显示 5002 GitLab is taking too much time to respond
sudo docker run --detach \
    --hostname gitlab.example.com \
    --publish 443:443 --publish 80:80 --publish 22:22 \
    --name gitlab \
    --restart always \
    --volume /srv/gitlab/config:/etc/gitlab \
    --volume /srv/gitlab/logs:/var/log/gitlab \
    --volume /srv/gitlab/data:/var/opt/gitlab \
    gitlab/gitlab-ce:latest
```



### gitlab基本操作流程

### gitlabCICD配置

