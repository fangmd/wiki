---
sidebar_position: 3
---

# CentOS

> 下面内容基于 CentOS 7.9.2009

1. [VMware 虚拟机安装 CentOS 教程](https://www.jianshu.com/p/fd79fdea8224)

# 初始化

```
# 更新库
sudo yum update -y

# 安装常用工具
sudo yum install -y wget vim net-tools
```


# 系统启停

```
sudo shutdown
reboot
```

# 安装 ifconfig

```
sudo yum install net-tools -y
```

ifconfig 查看 CentOS ip 地址

## 防火墙设置

端口放行:

```
sudo firewall-cmd --zone=public --add-port=8080/tcp --permanent
sudo firewall-cmd --zone=public --add-port=50000/tcp --permanent
sudo firewall-cmd --zone=public --add-port=9000/tcp --permanent

systemctl reload firewalld
```
