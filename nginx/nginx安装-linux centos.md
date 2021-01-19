# nginx安装-Linux centos

## 添加yum源

```bash
sudo rpm -ivh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
```

## 安装nginx

```bash
sudo yum install -y nginx
```

## 设置开机启动

```bash
sudo systemctl enable nginx
```

## 手动启动

```bash
sudo systemctl start nginx
```

## 重新加载

```bash
sudo systemctl reload nginx
```

## 配置文件

主配置文件 /etc/nginx/nginx.conf
默认配置文件 /etc/nginx/conf.d/default.conf

通常修改默认配置文件即可

```bash
sudo vim /etc/nginx/conf.d/default.conf
```

> 在主配置文件中引用其他配置文件

```nginx
include /usr/local/nginx/conf/default.d/*.conf;
```

## web目录

```bash
/usr/share/nginx/html
```

## 常见问题

### 转发请求至外网可以，转发至本机不行

```bash
setsebool -P httpd_can_network_connect 1
```

### 请求体过长

```nginx
client_max_body_size 20M;
```

### 找不到配置文件

```bash
nginx -t
```

## 遗留问题

想把web目录迁移到/home/admin/domain/nginx，提示402

