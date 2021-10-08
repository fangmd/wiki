---
sidebar_position: 1
---


# dotenv 环境变量管理

## Install

```
yarn add dotenv

yarn add -D dotenv
```

## 初始化

创建 `.env`

```
# 服务模式
NODE_ENV=development
# 服务运行的端口
SERVER_PORT=8080
```

`server.ts`

```
import dotenv from "dotenv"

// initialize configuration
dotenv.config()
console.log(process.env.NODE_ENV)
```
