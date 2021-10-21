---
sidebar_position: 7
---

# Docker

## 容器

重启容器：

```
docker restart [container]
```

### 进入/退出

进入：

```
docker exec -it [容器id] bash
```

退出:

```
exit
```

## logs

```
docker logs [OPTIONS] CONTAINER
```

查看容器实时日志:

```
docker logs -f [容器Id]
```

## 安装

### CentOS

```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2

sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

sudo yum install docker-ce -y
```

启动 docker: `systemctl` 是进程管理服务命令

```
systemctl start docker
systemctl enable docker
```

重启:

```
systemctl restart docker
```

安装 docker-compose:

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```

### 权限问题

```
sudo groupadd docker
# 把当前用户添加到 docker 组里面
sudo usermod -aG docker ${USER}
# 更新权限
newgrp docker
```

## 配置阿里云镜像源

- [https://cr.console.aliyun.com/cn-beijing/instances/mirrors](https://cr.console.aliyun.com/cn-beijing/instances/mirrors)

