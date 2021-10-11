---
sidebar_position: 6
---

# k8s

Kubernetes: 一个部署镜像的平台。可以用来操作多台机器调度部署镜像，大大地降低了运维成本。

使用 k8s 管理集群。集群就会存在 Master 节点和 Node 节点。通常 Node 节点才是我们业务服务器。

## 安装

### 前期准备

安装些必备组件。 vim 是 Linux 下的一个文件编辑器； wget 可以用作文件下载使用； ntpdate 则是可以用来同步时区:

```
yum install vim wget ntpdate -y
```

k8s 会创建防火墙规则，导致防火墙规则重复，所以这里先关闭防火墙:

```
systemctl stop firewalld & systemctl disable firewalld
```

>关闭 防火墙, swap, selinux 原因：https://www.zhihu.com/question/374752553

关闭 swap:

```
#临时关闭
swapoff -a
# 永久关闭
vi /etc/fstab

注释：/dev/mapper/centos-swap swap ...
```

关闭 Selinux：

```
# 暂时关闭 selinux
setenforce 0

# 永久关闭
vi /etc/sysconfig/selinux
# 修改以下参数，设置为disable
SELINUX=disabled
```

使用 ntpdate 同步时间:

```
# 统一时区，为上海时区
ln -snf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
bash -c "echo 'Asia/Shanghai' > /etc/timezone"

# 统一使用阿里服务器进行时间更新
ntpdate ntp1.aliyun.com
```

### 安装 Docker

重启 docker 并且查看 docker 状态：

```
systemctl restart docker && systemctl status docker
```

### 安装 Kubernetes 组件

将安装源更换为为国内的阿里云源：

```
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=http://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=0
repo_gpgcheck=0
gpgkey=http://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
        http://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF
```

```
yum install -y kubelet kubeadm kubectl
# 启动kubelet
systemctl enable kubelet && systemctl start kubelet
```

>kubelet 是 Kubernetes 中的核心组件。它会运行在集群的所有节点上，并负责创建启动服务容器 kubectl 则是Kubernetes的命令行工具。可以用来管理，删除，创建资源 kubeadm  则是用来初始化集群，子节点加入的工具。


重启：

```
service kubelet restart
```

### Master 节点安装

使用 `hostnamectl` 设置主机名称为 `master`:

>hostnamectl 是 CentOS7 新命令

```
hostnamectl set-hostname  master
```

修改 `/etc/hosts`:  master

```
vim /etc/hosts
# 本机ip master
192.168.79.5 master
```

#### 配置 Kubernetes 初始化文件

```
kubeadm config print init-defaults > init-kubeadm.conf
```

`vim init-kubeadm.conf`, 做以下修改

1. 更换 Kubernetes 镜像仓库为阿里云镜像仓库，加速组件拉取
2. 替换 ip 为自己主机 ip 
3. 配置 pod 网络为 flannel 网段

```
# imageRepository: k8s.gcr.io 更换k8s镜像仓库
imageRepository: registry.cn-hangzhou.aliyuncs.com/google_containers
# localAPIEndpointc，advertiseAddress为master-ip ，port默认不修改
localAPIEndpoint:
  advertiseAddress: 192.168.56.101  # 此处为master的IP
  bindPort: 6443
# 配置子网络
networking:
  dnsDomain: cluster.local
  serviceSubnet: 10.96.0.0/12
  podSubnet: 10.244.0.0/16	# 添加这个
```

使用 kubeadm 拉取我们的默认组件镜像:

```
kubeadm config images pull --config init-kubeadm.conf
```

#### 初始化 Kubernetes

```
kubeadm init --config init-kubeadm.conf
```

如果失败了，可以 `rm -rf $HOME/.kube/config && rm -rf /etc/kubernetes/manifests/`, `kubeadm reset` 重置，然后再执行。

成功后的图片:

![](/img/software/k8smaster-init.png)

保存下成功信息，后面会用到:

```
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.79.6:6443 --token abcdef.0123456789abcdef \
	--discovery-token-ca-cert-hash sha256:087679146a3330b200e1509ddc095d1bda0fd02feab8524ee87ec896932f559b
```

执行命令: 配置 kubectl 工具

```
  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

##### 遇到问题： kubelet.service fail to start up

`vim /etc/docker/daemon.json`

```
{
  "exec-opts": ["native.cgroupdriver=systemd"]
}
```

`sudo systemctl restart docker`

然后重新 init


#### 安装 Flannel

Flannel: 通过创建一个虚拟网络，让不同节点下的服务有着全局唯一的IP地址，且服务之前可以互相访问和连接。

下载配置文件:

```
wget http://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

从配置文件中找到 flannel 镜像(initContainers)，使用 docker 拉取:

```
docker pull quay.io/coreos/flannel:v0.14.0
```

使用 kubectl apply 命令加载服务:

```
kubectl apply -f kube-flannel.yml
```

查看启动情况：

```
kubectl get nodes
```

### Node节点配置

在 Node 节点机子上执行一遍：上面的前期准备，安装 Docker, 安装 Kubernetes 组件

设置 hostname:

```
hostnamectl set-hostname node1
```

#### 拷贝 Master 节点配置文件

在 master 机器上发送配置文件到 node 机器

```
scp $HOME/.kube/config root@node的ip:~/
```

在 Node 机器上, 归档配置文件：

```
mkdir -p $HOME/.kube
sudo mv $HOME/config $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

#### 加入 Master 节点

下面命令是，master 节点运行成功后获取的：

```
kubeadm join 192.168.79.6:6443 --token abcdef.0123456789abcdef \
	--discovery-token-ca-cert-hash sha256:6efa044044a224f229561ff898e9f0773fa67e337ceb5583ef7636d94628eff2
```

也可以重新在 master 机器上获取:

```
kubeadm token create --print-join-command
```

##### 问题 /etc/kubernetes/pki/ca.crt already exists

先 `kubeadm reset`


#### 安装 Flannel

Node 节点安装 Flannel 方式和 master 节点一样。

## 问题

1. cgroupDriver 问题

查看 docker cgroupDriver: 默认是 cgroupfs，需要修改成 systemd 和 k8s 一样。

```
docker info |grep -i cgroup
```

k8s地址: `/var/lib/kubelet/config.yaml`， 默认是 systemd

## ingress

对请求做路径分流。




## 命令

查看 Pod 信息

```
kubectl get pod
```

查看 Service 列表:

```
kubectl get svc
```



