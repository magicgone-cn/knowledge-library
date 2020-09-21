## 安装yum管理工具

```bash
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

## 添加yum源

```bash
sudo yum-config-manager --add-repo {url}
```

## 查看版本

```bash
sudo yum list docker-ce --showduplicates
```

## 删除

```bash
sudo yum remove xxx
```

