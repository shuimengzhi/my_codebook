# install containerd.io
go to this websit that view list
```
https://download.docker.com/linux/centos/7/x86_64/stable/Packages
```
install latest version
```
dnf install https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.6-3.3.el7.x86_64.rpm
```
# install docker
reference this link 
```
https://docs.docker.com/engine/install/centos/
```
## uninstall old version 
```
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```
## Install using the repository
```
yum update -y
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```
## Install Docker Engine 
```
sudo yum install docker-ce docker-ce-cli
```
## Create /etc/docker
```
mkdir /etc/docker
```
# Set up the Docker daemon
```
cat > /etc/docker/daemon.json <<EOF
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
EOF
```
```
mkdir -p /etc/systemd/system/docker.service.d
```
# Restart Docker
```
systemctl daemon-reload
systemctl restart docker
```
If you want the docker service to start on boot, run the following command
```
sudo systemctl enable docker
```





# Installing kubeadm 
```
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
```
```
sudo sysctl --system
```
## Set up Kubernetes repository 
vim /etc/yum.repos.d/kubernetes.repo
```
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
```
## Set SELinux in permissive mode (effectively disabling it)
```
sudo setenforce 0
sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
```
## 关闭交换分区
```
f
```
## 查看是否关闭
```
free -m
```
## install kubelet kubeadm kubectl
```
sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
sudo systemctl enable --now kubelet
systemctl enable kubelet.service
```
## 配置主机名
```
hostnamectl set-hostname 主机名
```
```
kubeadm init --kubernetes-version=1.19.1  \
--apiserver-advertise-address=172.16.183.200   \
--image-repository registry.aliyuncs.com/google_containers  \
--service-cidr=10.10.0.0/16 --pod-network-cidr=10.122.0.0/16
```