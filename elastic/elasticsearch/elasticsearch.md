```sh
rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```

```sh
[elasticsearch-7.x]
name=Elasticsearch repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```

```sh
sudo yum install elasticsearch 
sudo dnf install elasticsearch 
sudo zypper install elasticsearch 
```

```sh
sudo systemctl start elasticsearch.service
sudo systemctl stop elasticsearch.service
```

cd  /usr/share/elasticsearch/bin/

 ./elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.2.0/elasticsearch-analysis-ik-7.2.0.zip

文档： https://www.elastic.co/guide/en/elasticsearch/reference/current/rpm.html

