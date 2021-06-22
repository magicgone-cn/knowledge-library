# 安装

## 客户端

### centos 7

```sh
pip install shadowsocks
```

> /etc/shadowsocks/sslocal.json

```json
{
  "server": "服务端IP",
  "server_port": 8388,
  "local_address": "0.0.0.0",
  "local_port": 1080,
  "password": "密码",
  "timeout": 300,
  "method": "aes-256-cfb",
  "fast_open": false,
  "workers": 1
}
```

> /usr/lib/systemd/system/sslocal.service or /etc/systemd/system/sslocal.service

```conf
[Unit]
Description=Shadowsocks-sslocal

[Service]
TimeoutStartSec=0
ExecStart=/usr/bin/sslocal -c /etc/shadowsocks/sslocal.json

[Install]
WantedBy=multi-user.target
```

```sh
systemctl start sslocal
systemctl enable sslocal
```

> 验证

```sh
curl --socks5 127.0.0.1:1080 http://httpbin.org/ip
```

### docker

***测试时发现，和iptable有一定的冲突，绕过了firewalld，且firewalld打开才可以正常使用，具体原因待排查***

```sh
docker container run -d -p 1080:1080 -p 1080:1080/udp -v /etc/shadowsocks/sslocal.json:/etc/shadowsocks/sslocal.json --name sslocal shadowsocks/shadowsocks-libev /bin/sh -c 'exec ss-local -c /etc/shadowsocks/sslocal.json'
```

## 服务端

```bash
docker run -e PASSWORD=password -e METHOD=aes-256-cfb -p 8388:8388 -p 8388:8388/udp -d --restart always --name shadowsocks shadowsocks/shadowsocks-libev
```

