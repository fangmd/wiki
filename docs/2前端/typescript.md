---
sidebar_position: 1
---


# Typescript

安装 ts

```
npm i --save-dev typescript nodemon ts-node @types/node
```

- `nodemon ts-node @types/node` 这 3 个在 Typescript 项目中通常也是需要的。



## tsc

tsc 是 Typescript 的 cli 工具。

- `tsc --init` 初始化 ts 项目。主要功能是创建 `tsconfig.json` 文件


## tsconfig.json

常用配置

- `"outDir": "./dist",`, 设置编译输出位置