# tunnelto

> 将本地服务暴露到公网的命令行工具

---

## :zap: 核心功能

- 一条命令将本地端口暴露到公网
- 生成可分享的公网 URL
- 无需注册账号，开箱即用
- 支持任意协议（HTTP/WebSocket/TCP）

---

## :gear: 工作原理

> tunnelto 通过自有服务器建立隧道，将公网请求转发到你的本地服务。适合快速分享 demo、测试 webhook、演示还未部署的功能。

---

## :package: 安装

```bash
# macOS
brew install tunnelto

# 或下载二进制
curl -Ls https://bin.thermion.one/tunnelto.sh | bash
```

---

## :keyboard: 使用方式

### 基本用法

```bash
# 将本地 3000 端口暴露到公网
tunnelto --port 3000

# 指定子域名（可选）
tunnelto --port 3000 --subdomain my-demo
```

> 执行后会返回一个公网 URL，格式类似：`https://xxxxx.tunnelto.dev`

### 指定端口

```bash
tunnelto --port 8080
```

### 查看帮助

```bash
tunnelto --help
```

---

## :eyes: 适用场景

| 场景 | 说明 |
|------|------|
| 快速分享本地 demo | 给同事/客户展示还未部署的新功能 |
| 测试 webhook | 接收第三方服务的回调 |
| 移动端调试 | 在手机上测试本地 API |
| 演示分享 | Conference 或 meetup 中展示实时 demo |

---

## :scales: 对比 ngrok

| 特性 | tunnelto | ngrok |
|------|:--------:|:-----:|
| 注册账号 | :x: 不需要 | :white_check_mark: 需要 |
| 自定义域名 | :x: 不支持 | :white_check_mark: 支持（付费） |
| 协议支持 | HTTP/TCP | HTTP/TCP |
| 界面 | 极简 | 丰富 |
| 开源 | :white_check_mark: 是 | :x: 否 |

---

## :warning: 注意事项

> - 隧道稳定性依赖 tunnelto 服务端
> - 公网 URL 临时有效，适合开发和演示
> - 不适合生产环境使用

---

## :link: 相关工具

- [[02-zoxide]] — 智能目录跳转
