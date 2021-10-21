---
sidebar_position: 4
---

# Jenkins

## 安装

### CentOS

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

## NodeJS

NodeJS 插件，提供 node 环境

1. 安装：NodeJS 插件后，在 Global Tool Configuration => NodeJS => 新增 NodeJS 添加 Node 环境。

2. 使用：在构建任务的 `Build Environment` 中添加 NodeJS 环境

## 构建时从 git 拉代码

1. 在电脑中生成 ssh-key

```
ssh-keygen -t rsa -C "janlay884181317@gmail.com"
```

2. 将公钥注册到 github/gitlab
3. 在 jenkins 中添加私钥

## 操作 Docker login 时添加账号

`docker login -u "用户名" -p "密码" 172.16.81.7:8082`

在任务里面添加账号密码: `Build Environment` -> `Use secret text ...` -> `Username and password(separated)`

![](/img/software/jenkins-docker-login.png)

修改 login 命令:

```
docker login -u $DOCKER_LOGIN_USERNAME -p $DOCKER_LOGIN_PASSWORD 192.168.79.5:8082
```

## Jenkins 调用 docker 出现 Unix Socket 权限问题

```
sudo groupadd docker          #新增docker用户组
sudo gpasswd -a jenkins docker  #将当前用户添加至docker用户组
newgrp docker                 #更新docker用户组
```

加入后重启 jenkins: `sudo service jenkins restart`


## Pipeline 操作远程服务器

需要使用 SSH Agent 插件。通过这个插件设置 ssh

1. 安装插件 `SSH Agent`
2. 添加 credential
3. 在 pipeline 中使用

scp 发送文件到服务器，在服务器解压文件

```
sshagent (credentials: ["63b8204c-44a2-4c06-9288-e569e48b54b1"]) {
     sh "scp build.tar.gz root@123.56.139.231:/root/yt/wiki" 
     sh "ssh -o StrictHostKeyChecking=no root@123.56.139.231 \"tar -xvf /root/yt/wiki/build.tar.gz -C /root/yt/wiki/\""
}
```