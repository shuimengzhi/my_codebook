# Install docker

# Installing kubeadm
```
https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
```

# Centos
```
cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-\$basearch
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kubelet kubeadm kubectl
EOF
```

# Set SELinux in permissive mode (effectively disabling it)
```
sudo setenforce 0
```
vim /etc/selinux/config
```
SELINUX=disabled
```
sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes

sudo systemctl enable --now kubelet

关闭交换分区
```
swapoff -a
```
查看是否关闭
```
free -m
```
配置主机名
```
hostnamectl set-hostname 主机名
```
配置时间同步
```
yum install chrony -y
vim /etc/chrony.conf
pool 2.centos.pool.ntp.org iburst
systemctl restart chronyd
chronyc sources -v #查看同步状态
```
# Install cfssl
```
wget  https://pkg.cfssl.org/R1.2/cfssl_linux-amd64 -O  /usr/bin/cfssl
wget  https://pkg.cfssl.org/R1.2/cfssljson_linux-amd64 -O /usr/bin/cfssl-json
wget  https://pkg.cfssl.org/R1.2/cfssl-certinfo_linux-amd64 -O /usr/bin/cfssl-certinfo 
chmod +x /usr/bin/cfssl*
cd /opt/
mkdir certs
cd certs
cfssl print-defaults config > config.json
cfssl print-defaults csr > csr.json
```
vim csr.json
```
{
    "CN": "master.k8s",
    "hosts": [
    ],
    "key": {
        "algo": "rsa",
        "size": 2048
    },
    "names": [
        {
            "C": "CN",
            "L": "Xiamen",
            "ST": "Fujian",
            "O": "master"
        }
    ],
    "ca": {
      "expiry": "175200h"
    }
}
```
vim config.json
```
{
    "signing": {
        "default": {
            "expiry": "175200h"
        },
        "profiles": {
            "server": {
                "expiry": "175200h",
                "usages": [
                    "signing",
                    "key encipherment",
                    "server auth"
                ]
            },
            "client": {
                "expiry": "175200h",
                "usages": [
                    "signing",
                    "key encipherment",
                    "client auth"
                ]
            },
             "peer": {
                "expiry": "175200h",
                "usages": [
                    "signing",
                    "key encipherment",
                    "server auth",
                    "client auth"
                ]
            }
        }
    }
}
```
## 生成 CA 密钥（ca-key.pem）和证书(ca.pem)

```
cfssl gencert -initca csr.json | cfssl-json -bare ca
```
vim etcd-peer-csr.json
```
{
    "CN": "k8s-etcd",
    "hosts": [
        "172.16.183.101",
        "172.16.183.102"
    ],
    "key": {
        "algo": "rsa",
        "size": 2048
    },
    "names": [
        {
            "C": "CN",
            "L": "Xiamen",
            "ST": "Fujian",
            "O": "master"
        }
    ]
}
```
```
cfssl gencert -ca=ca.pem -ca-key=ca-key.pem \
     --config=config.json -profile=peer \
     etcd-peer-csr.json | cfssl-json -bare etcd-peer

```
签发证书完以后把ca.pem  etcd-peer-key.pem  etcd-peer.pem复制到部署etcd的服务器上
# Init cluster
查看默认初始化配置
kubeadm config print init-defaults
config.yml
```
apiVersion: kubeadm.k8s.io/v1beta2
bootstrapTokens:
- groups:
  - system:bootstrappers:kubeadm:default-node-token
  token: abcdef.0123456789abcdef
  ttl: 24h0m0s
  usages:
  - signing
  - authentication
kind: InitConfiguration
localAPIEndpoint:
  advertiseAddress: 172.16.183.100
  bindPort: 6443
nodeRegistration:
  criSocket: /var/run/dockershim.sock
  name: k8s-master
  taints:
  - effect: NoSchedule
    key: node-role.kubernetes.io/master
---
apiServer:
  timeoutForControlPlane: 4m0s
apiVersion: kubeadm.k8s.io/v1beta2
certificatesDir: /etc/kubernetes/pki
clusterName: kubernetes
controllerManager: {}
dns:
  type: CoreDNS
etcd:
  local:
    dataDir: /var/lib/etcd
imageRepository: k8s.gcr.io
kind: ClusterConfiguration
kubernetesVersion: v1.19.0
networking:
  dnsDomain: 172.16.183.103
  podSubnet: 10.100.0.1/24
  serviceSubnet: 10.96.0.0/12
scheduler: {}
---
apiVersion: kubeproxy.config.k8s.io/v1alpha1
kind: KubeProxyConfiguration
featureGates:
  SupportIPVSProxyMode: true
mode: ipvs
```

kubeadm init --config=kubeadm-config.yaml --upload-certs | tee kubeadm-init.log


# Install etcd
禁用etcd这个用户
useradd -s /sbin/nologin -M etcd

cd /root/
wget https://github.com/etcd-io/etcd/releases/download/v3.3.25/etcd-v3.3.25-linux-amd64.tar.gz
tar xvf etcd-v3.3.25-linux-amd64.tar.gz
rm -f etcd-v3.3.25-linux-amd64.tar.gz
mv etcd-v3.3.25-linux-amd64/ /opt/etcd-v3.3.25
cd /opt
ln -s etcd-v3.3.25/ etcd

mkdir -p /opt/etcd/certs /data/etcd /data/logs/etcd-server

chown -R etcd:etcd /opt/etcd-v3.3.25/
chown -R etcd:etcd /data/etcd
chown -R etcd:etcd /data/logs/etcd-server
<!--安装进程管理器-->
pip3 install pip --upgrade
pip install supervisor

systemctl start supervisord.service
systemctl enable supervisord.service

cd /opt/etcd && vim etcd-server-startup.sh
demo
```
#!/bin/sh
./etcd  --name ${ETCD_NAME} \\
        --data-dir=/var/lib/etcd \\
        --listen-peer-urls https://${INTERNAL_IP}:2380 \\
        --listen-client-urls https://${INTERNAL_IP}:2379,https://127.0.0.1:2379 \\
        --initial-advertise-peer-urls https://${INTERNAL_IP}:2380 \\
        --advertise-client-urls https://${INTERNAL_IP}:2379 \\
        --initial-cluster controller-0=https://10.240.0.10:2380,controller-1=https://10.240.0.11:2380,controller-2=https://10.240.0.12:2380 \\
        --trusted-ca-file=/etc/etcd/ca.pem \\
        --cert-file=/etc/etcd/kubernetes.pem \\
        --key-file=/etc/etcd/kubernetes-key.pem \\
        --client-cert-auth \\
        --peer-cert-file=/etc/etcd/kubernetes.pem \\
        --peer-key-file=/etc/etcd/kubernetes-key.pem \\
        --peer-trusted-ca-file=/etc/etcd/ca.pem \\
        --peer-client-cert-auth \\
        --initial-cluster-token etcd-cluster-0 \\
        --initial-cluster-state new \\
        --log-output stdout
```


node1
```
#!/bin/sh
./etcd  --name node1-etcd \\
        --data-dir=/data/etcd/node1-etcd \\
        --listen-peer-urls https://172.16.183.101:2380 \\
        --listen-client-urls https://172.16.183.101:2379,https://127.0.0.1:2379 \\
        --initial-advertise-peer-urls https://172.16.183.101:2380 \\
        --advertise-client-urls https://172.16.183.101:2379 \\
        --initial-cluster node1-etcd=https://172.16.183.101:2380,node2-etcd=https://172.16.183.102:2380,node3-etcd=https://172.16.183.104:2380 \\
        --trusted-ca-file=./certs/ca.pem \\
        --cert-file=./certs/etcd-peer.pem \\
        --key-file=./certs/etcd-peer-key.pem \\
        --client-cert-auth \\
        --peer-cert-file=./certs/etcd-peer.pem \\
        --peer-key-file=./certs/etcd-peer-key.pem \\
        --peer-trusted-ca-file=./certs/ca.pem \\
        --peer-client-cert-auth \\
        --initial-cluster-token etcd-cluster-0 \\
        --initial-cluster-state new \\
        --log-output stdout
```