# 安装

## 修改配置(不懂)

```sh
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

sysctl --system
```

已经设置为disable,可以跳过

```bash
# Set SELinux in permissive mode (effectively disabling it)
sudo setenforce 0
sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
```

## 关闭swap

```sh
# 临时关闭
swapoff -a
# 永久关闭
vim /etc/fstab
# 删除swap的那一行
```

## 修改docker配置

> /etc/docker/daemon.json

```json
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2",
  "storage-opts": [
    "overlay2.override_kernel_check=true"
  ]
}
```

## 设置yum源

```bash
cat > /etc/yum.repos.d/kubernetes.repo <<EOF
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=0
repo_gpgcheck=0
EOF
```

```bash
sudo yum install -y kubelet kubeadm kubectl
sudo systemctl enable --now kubelet
```

## 使用代理(可选)

下载gcr.io的镜像需要科学上网

### socks5

```sh
mkdir -p /etc/systemd/system/docker.service.d
touch /etc/systemd/system/docker.service.d/http-proxy.conf
vim /etc/systemd/system/docker.service.d/http-proxy.conf
```

> /etc/systemd/system/docker.service.d/http-proxy.conf

```conf
[Service]
Environment="HTTP_PROXY=socks5://ip:port/"
Environment="HTTPS_PROXY=socks5://ip:port/"
# 过滤不使用代理
Environment="NO_PROXY=*.aliyuncs.com,*.docker.io,*.docker.com"
```

```sh
systemctl daemon-reload
systemctl restart docker
```

验证

```sh
systemctl show --property=Environment docker
```

## 拉取镜像(可选)

kubeadm init也可以拉取所需镜像，提前手工拉取，可以验证获取镜像的网络

```bash
kubeadm config images pull
```

## 初始化集群

```bash
kubeadm init --pod-network-cidr=10.244.0.0/16
```

## 初始化网络(CNI)

文件无法访问，可以先下载到本地

```bash
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

## 加入节点

```bash
kubeadm join 192.168.13.86:6443 --token xxxx --discovery-token-ca-cert-hash sha256:xxxx
```

> 相关token信息可以通过以下方式在主节点生成

```bash
kubeadm token create --print-join-command
```

# Context

> ~/.kube/config

# 调试

```bash
kubectl run --rm -it busybox --image busybox --restart=Never
```