---
sidebar_position: 5
---

# Nexus

制品库：存放制品并且做版本管理

可以管理 docker 镜像，npm 包，maven库 ...

## CentOS Install

```
wget https://dependency-fe.oss-cn-beijing.aliyuncs.com/nexus-3.29.0-02-unix.tar.gz

tar -zxvf ./nexus-3.29.0-02-unix.tar.gz
```

`nexus-3.29.0-02/bin`

```
cd nexus-3.29.0-02/bin
./nexus start
```

>启动需要一点时间(1min)

nexus 默认端口：8081

设置防火墙: 公开 8081

```
firewall-cmd --zone=public --add-port=8081/tcp --permanent
firewall-cmd --zone=public --add-port=8082/tcp --permanent

systemctl reload firewalld
```

>8082 给添加仓库的时候使用

访问 IP:8081.

## 配置 Nexus

点击右上角登录, 做管理员账号初始化配置，账号: admin, 密码在下面地址

```
cat ./sonatype-work/nexus3/admin.password

c349b170-b663-4a8e-8b9d-f0e90d8e1d58
```

## 添加一个 Docker 仓库

端口设置: 8082

## Docker 登入 Nexus

私服如果是 Http，需要再做个配置:

```
vi /etc/docker/daemon.json
```

添加私服地址: (8082 是 docker 仓库的端口)

```
{
  "insecure-registries" : [
    "172.16.81.7:8082"
  ],
}
```

重启 Docker

```
systemctl restart docker
```

登录：`docker login 192.168.79.5:8082`