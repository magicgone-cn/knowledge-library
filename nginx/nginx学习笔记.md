# 转发websocket

```
map $http_upgrade $connection_upgrade {
    default upgrade;
    '' close;
}

location = /ybkf/websocket {
    proxy_pass http://hsa-ybkf:8080;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection $connection_upgrade;
}
```

# 配置ssl

```
ssl_certificate /etc/nginx/ssl/sb.sinobest.cn.crt;
ssl_certificate_key /etc/nginx/ssl/sb.sinobest.cn.key;
```