## 地址中去掉某一段

- 一个种方案是proxy_pass后面加根路径`/`

```nginx
location /v1/ {
	proxy_pass http://localhost/;
}
# proxy_pass结尾包含 `/`,可以将order后面的内容拼接到服务地址下面即
# http://localhost/v1/abc ==> http://localhost/abc 达到去除v1的目的
```

- 另一种方案是使用`rewrite` **(需要测试，暂做记录)**

```nginx
location /v1/ {
	rewrite ^/v1/(.*)$ /$1 break;
	proxy_pass http://localhost;
}
# proxy_pass 后面没有 `/`
```

