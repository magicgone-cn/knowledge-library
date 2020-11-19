# 使用docker启动jenkins

```bash
docker run -u root -dp 8080:8080 -e TZ=Asia/Shanghai -v jenkins-data:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -v /usr/bin/docker-compose:/usr/bin/docker-compose:ro -v jenkins-m2:/root/.m2 --name jenkins jenkinsci/blueocean
```

注意宿主机需要安装docker-compose，安装方式如下

> 版本号根据需要修改

```bash
curl -L https://github.com/docker/compose/releases/download/1.26.2/docker-compose-Linux-x86_64 > /usr/bin/docker-compose
chmod 755 /usr/bin/docker-compose
```
