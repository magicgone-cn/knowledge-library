# redis安装-Linux centos

## 添加仓库

```bash
sudo yum install epel-release
sudo yum update
```

## 安装redis

```bash
sudo yum install -y redis
```

## 设置开机启动

```bash
sudo systemctl enable redis
```

## 手动启动

```bash
sudo systemctl start redis
```

## 重新启动

```bash
sudo systemctl restart redis
```

## 查看状态

```bash
sudo systemctl status redis
```

## 配置文件

```bash
sudo systemctl vim /etc/redis.conf
```



# 常见问题

## redis不支持config

```java
@Bean
public static ConfigureRedisAction configureRedisAction() {
    return ConfigureRedisAction.NO_OP;
}
```



