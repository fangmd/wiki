---
sidebar_position: 1
---

# Linux 常用命令

## 查看端口占用情况

```
lsof -i:端口号
```

```
➜ lsof -i:8001
COMMAND   PID   USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
node    10462 double  240u  IPv6 0x526cff16c9d9b307      0t0  TCP *:vcom-tunnel (LISTEN)
```

## 查看端口是否通

```
telnet 10.0.250.3 80
```

## 发送文件到远程服务器 

```
scp [本地文件] [远程用户名]@[远程ip]:[文件地址]

scp /Users/double/projects/docker-examples/gitlab/docker-compose.yml double@192.168.79.8:~/gitlab/
```

