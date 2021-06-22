# 安装

## docker

### 服务端

```bash
docker run -d --name some-clickhouse-server --ulimit nofile=262144:262144 -v clickhouse-data:/var/lib/clickhouse yandex/clickhouse-server
```

8123 http

9000 native client

### 客户端

```bash
docker run -it --rm --link some-clickhouse-server:clickhouse-server -v clickhouse-client-data:/var/lib/clickhouse yandex/clickhouse-client --host clickhouse-server
```

