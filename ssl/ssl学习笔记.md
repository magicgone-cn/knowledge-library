```bash
# 1.生成私钥
$ openssl genrsa -out server.key 2048

# 2.生成 CSR (Certificate Signing Request)
$ openssl req \
    -subj "/C=CN/ST=Tianjin/L=Tianjin/O=Mocha/OU=Mocha Software/CN=test1.sslpoc.com/emailAddress=test@mochasoft.com.cn" \
    -new \
    -key server.key \
    -out server.csr

# 3.生成自签名证书
$ openssl x509 \
    -req \
    -days 3650 \
    -in server.csr \
    -signkey server.key \
    -out server.crt
```

