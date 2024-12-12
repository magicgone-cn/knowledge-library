> 启动命令

```bash
docker run -v d:/redis/conf:/usr/local/etc/redis -v d:/redis/data:/data --name redis -dp 6379:6379 redis redis-server /usr/local/etc/redis/redis.conf --requirepass a --appendonly yes
```
