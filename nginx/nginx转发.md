## 地址中去掉某一段

- 一个种方案是proxy_pass后面加根路径`/`

```nginx
location /v1/ {
	proxy_pass http://localhost/;
}
# proxy_pass结尾包含 `/`,可以将order后面的内容拼接到服务地址下面即
# http://localhost/v1/abc ==> http://localhost/abc 达到去除v1的目的
```

- 另一种方案是使用`rewrite` 

```nginx
location /v1/ {
	rewrite ^/v1/(.*)$ /$1 break;
	proxy_pass http://localhost;
}
# proxy_pass 后面没有 `/`
```

## 负载均衡

```nginx
upstream upstream_name{
        server 192.168.0.28:8001;
        server 192.168.0.28:8002;
}
server {
        listen       8080;
        server_name  localhost;

        location / {
            proxy_pass http://upstream_name;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }
```

### 策略

- 轮询
- 权重
- 最小连接
- ip hash

### 健康检查

- 被动健康检查

- 主动健康检查（淘宝开源实现nginx_upstream_check_module模块）

  ```nginx
  upstream cluster2 {
          # simple round-robin
          server 192.168.0.3:80;
          server 192.168.0.4:80;
  
          check interval=3000 rise=2 fall=5 timeout=1000 type=http;
          check_keepalive_requests 100;
          check_http_send "HEAD / HTTP/1.1\r\nConnection: keep-alive\r\n\r\n";
          check_http_expect_alive http_2xx http_3xx;
      }
  
      server {
          listen 80;
  
          location /1 {
              proxy_pass http://cluster1;
          }
  
          location /2 {
              proxy_pass http://cluster2;
          }
  
          location /status {
              check_status;
  
              access_log   off;
              allow SOME.IP.ADD.RESS;
              deny all;
          }
      }
  ```

  

