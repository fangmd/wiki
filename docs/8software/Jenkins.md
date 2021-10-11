---
sidebar_position: 4
---

# Jenkins

## NodeJS

NodeJS 插件，提供 node 环境

1. 安装：NodeJS 插件后，在 Global Tool Configuration => NodeJS => 新增NodeJS 添加 Node 环境。

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
