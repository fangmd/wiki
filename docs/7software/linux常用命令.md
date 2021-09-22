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
