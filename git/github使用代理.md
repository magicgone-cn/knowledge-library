# http

> 只对github.com

git config --global http.https://github.com.proxy socks5://127.0.0.1:1080

> 取消代理

git config --global --unset http.https://github.com.proxy)

# ssh

> ~/.ssh/config

```
Host github.com
ProxyCommand connect -S 127.0.0.1:1080 %h %p
```
