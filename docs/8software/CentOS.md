---
sidebar_position: 3
---

# CentOS

> 下面内容基于 CentOS 7.9.2009

1. [VMware 虚拟机安装 CentOS 教程](https://www.jianshu.com/p/fd79fdea8224)


`sudo yum update -y`

# 系统启停

```
sudo shutdown
reboot
```

# 安装 ifconfig

```
yum install net-tools
```

ifconfig 查看 CentOS ip 地址

# 安装 Docker

```
yum install -y yum-utils device-mapper-persistent-data lvm2

sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
yum install docker-ce -y
```

启动 docker: `systemctl` 是进程管理服务命令

```
systemctl start docker
systemctl enable docker
```

## Jenkins 调用 docker 出现 Unix Socket 权限问题

```
sudo groupadd docker          #新增docker用户组
sudo gpasswd -a jenkins docker  #将当前用户添加至docker用户组
newgrp docker                 #更新docker用户组
```

加入后重启 jenkins: `sudo service jenkins restart`


## 配置个阿里云镜像源

- [https://cr.console.aliyun.com/cn-beijing/instances/mirrors](https://cr.console.aliyun.com/cn-beijing/instances/mirrors)

# 安装 Jenkins

```
yum install -y java

sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo --no-check-certificate

sudo vim /etc/yum.repos.d/epelfordaemonize.repo

[daemonize]
baseurl=https://download-ib01.fedoraproject.org/pub/epel/7/x86_64/
gpgcheck=no
enabled=yes

yum install jenkins -y
```

Jenkins 操作：

```
sudo service jenkins start
sudo service jenkins restart
sudo service jenkins stop
```

jenkins 位置：`/var/lib/jenkins`

## jenkins 端口放行

```
sudo firewall-cmd --zone=public --add-port=8080/tcp --permanent
sudo firewall-cmd --zone=public --add-port=50000/tcp --permanent

systemctl reload firewalld
```

访问 jenkins: `ip:8080`


