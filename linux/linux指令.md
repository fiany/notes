## linux指令

- more ss.sql
- less ss.sql
- cat /etc/shells  查看shell
- chsh -s zsh 切换shell

### 查找文件

#### find

find path [options] params

- 在指定目录查找文件
- -name 指定文件名
- -iname 不区分大小写

### 检索文件内容

#### grep: 查找文件中符合条件的字符串

grep [options] pattern file

#### | 管道操作符

- 可将命令连接起来，前一个指令的输出作为后一个指令的输入
- 只处理前一个命令正确输出，不处理错误输出
- 右边的命令必须能够接受标准输入流，否则传递过程中数据会被抛弃
- sed,awk,grep,cut,head,top,less,more,wc,join,sort,split等

### 对文件内容做统计

#### awk

awk [options] 'cmd' file

- 一次读取一行文本，按输入分隔符进行切片，切成多个组成部分
- 将切片直接保存在内建的变量中，$1,$2...($0表示行的全部)
- 支持对单个切片的判断，支持循环判断，默认分隔符为空格

```bash
# 常用操作
awk '{print $1,$4}' netstat.txt
awk '$1=="tcp" && $2==1 {print $0}' netstat.txt
awk '{enginearr[$1]++}END{for(i in enginearr)print i"\t"enginearr[i]}'
```

### 批量替换文本内容

#### sed

sed [option] 'sed command' filename

- stream editor 流编辑器
- 适合用于对文本的行内容进行处理

```bash
# 常用操作
sed -i 's/^Str/String/' replace.java
sed -i 's/\.$/\;/' replace.java
sed -i 's/Jack/me/g' replace.java
sed -i '/^ *$/d' replace.java # 删除空行
```



