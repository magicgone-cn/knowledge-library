## 编码格式调整

```
JAVA_OPTS = -Dfile.encoding=UTF8
```

> server.xml

```xml
<Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443"
           URIEncoding="UTF-8" />
```

