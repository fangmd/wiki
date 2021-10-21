---
sidebar_position: 8
---

# Gitlab

# 服务器配置

最低 2核/4g，swap2g
建议 4核/8g, swap4g


# 基本命令

查看状态:

```
gitlab-ctl status
```

启动:

```
gitlab-ctl start
```

重启:

```
gitlab-ctl restart
```

修改配置文件`gitlab.rb`, 后更新配置:

```
gitlab-ctl reconfigure
```

# 安装

## 使用 docker-compose 安装

`docker-compose.yml`:

```
version: "3"
services:
  gitlab-service:
    image: "gitlab/gitlab-ce:latest"
    restart: always
    hostname: "192.168.79.8"
    environment:
      TZ: 'Asia/Shanghai'
      GITLAB_OMNIBUS_CONFIG: |
          external_url 'http://192.168.79.8'
          nginx['listen_port'] = 80
          gitlab_rails['gitlab_shell_ssh_port'] = 9022
    ports:
      - "9000:80"
      - "9443:443"
      - "9022:22"
    volumes:
      - "/Users/double/soft/gitlab-data/config:/etc/gitlab"
      - "/Users/double/soft/gitlab-data/logs:/var/log/gitlab"
      - "/Users/double/soft/gitlab-data/data:/var/opt/gitlab"
```

解释：

1. `TZ: 'Asia/Shanghai'`: 设置时区
2. `ports`: 端口映射
3. `volumes`: 数据持久化
4. `gitlab_rails['gitlab_shell_ssh_port'] = 9022` 设置 ssh 端口。设置后，仓库页面的 clone ssh 会添加对应端口。


```
docker-compose up -d
```

## 初始化账号

1. 进入容器

2. 执行下面命令

```
gitlab-rails console -e production

user=User.where(id:1).first
user.password='root0224'
user.password_confirmation='root0224'
user.save!
```

账号：root/root0224
