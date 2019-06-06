### 安装步骤

> 参考 [阿里云教程](https://help.aliyun.com/document_detail/116727.html?source=5176.11533457&type=copy)

### 设置utf-8mb4字符集保存数据乱码

```bash
修改数据库配置文件/etc/my.cnf

character-set-server=utf8mb4
collation_server=utf8mb4_general_ci

重启MySQL（按照官方文档，这两个选项都是可以动态设置的，但是实际的经验是Server必须重启一下）
```



