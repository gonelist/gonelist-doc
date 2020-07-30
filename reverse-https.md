# 反向代理

反向代理的意思就是把 8000 端口的内容映射到一个 host
下面这个例子就是你可以通过访问 http://gonelist.cugxuan.cn 来达到访问 http://gonelist.cugxuan.cn:8000（http://cugxuan.cn:8000） 一样的效果

```
# 监听反向代理端口
upstream gonelist_pool {
    server 127.0.0.1:8000;
}
server {
    listen       80;
    server_name  gonelist.cugxuan.cn;

    location / {
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://gonelist_pool;
    }
}
```

# HTTPS

如果你的域名申请了 HTTPS 证书，可以通过 nginx 反向代理。**建议将** `config.json` 中的 `bind_global` 设置为 `false`（作用就是不会让外部通过 http 访问），然后修改 nginx 设置，以我的 `https://gonelist.cugxuan.cn`，腾讯云的免费证书为例

```
$ cd /etc/nginx/sites-enabled
$ vi gonelist.cugxuan.cn
```

在 nginx 配置文件中写入下面内容，然后 `$ nginx` 启动或 `$ nginx -s reload` 重启即可

```
server {
    listen       80;
    server_name  gonelist.cugxuan.cn;

    # 强制跳转 HTTPS
    rewrite ^(.*) https://$server_name$1 permanent;

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   html;
    }
}

# 监听反向代理端口
upstream gonelist_pool {
    server 127.0.0.1:8000;
}

server {
    listen 443;

    server_name gonelist.cugxuan.cn; #填写绑定证书的域名
    # 这一部分都是腾讯云的配置说明抄下来改路径，不同服务商请查询各家文档
    ssl on;
    ssl_certificate tecent-ssl/gonelist.cugxuan.cn/Nginx/1_gonelist.cugxuan.cn_bundle.crt;
    ssl_certificate_key tecent-ssl/gonelist.cugxuan.cn/Nginx/2_gonelist.cugxuan.cn.key;
    ssl_session_timeout 5m;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2; #按照这个协议配置
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;#按照这个套件配置
    ssl_prefer_server_ciphers on;

    location / {
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://gonelist_pool;
    }
}
```
