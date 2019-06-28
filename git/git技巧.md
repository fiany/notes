- 首次使用github的时候，第一次创建git仓库，如果创建了readme文件或者license文件，在pull的时候不能将代码拉到本地，使用命令 告诉 git 允许不相关历史合并。

```bash
git pull origin master --allow-unrelated-histories
```

- git回滚本地未提交的修改

```bash
git checkout .
```

- 强制拉取远程代码

```bash
git pull --force
```



- git克隆项目的时候指定 ssh-key

```bash
# 新建文件 ~/.ssh/config

Host gitee
    Hostname gitee.com
    User git
    IdentityFile ~/.ssh/id_rsa_xj # 指定文件位置
# 然后可以使用gitee替换git@gitee.com
git clone ssh://gitee/gzdianruan/carnetwork.git
```






