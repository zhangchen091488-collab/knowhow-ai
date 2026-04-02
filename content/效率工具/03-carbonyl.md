# Carbonyl

> 服务端浏览器市场的强力竞争者——在终端里运行完整 Chromium

---

## :crossed_swords: 核心定位

> Carbonyl 是一个专为命令行环境设计的 Chromium 浏览器，完全在无 GUI 的服务器或终端环境中运行。区别于传统"无头浏览器"，它支持完整的 CSS 动画、JavaScript 执行，甚至可以用 ASCII 字符在终端渲染页面。

---

## :rocket: 技术特点

### 性能优异

| 指标 | 说明 |
|------|------|
| 启动速度 | 比 Puppeteer/Playwright 快 2-3 倍 |
| 内存占用 | 更轻量，适合服务器环境 |
| 二进制大小 | 单文件，约 50-80MB |

### 完整的浏览器能力

- 完整的 CSS/JS 支持，包括 CSS 动画
- JavaScript 渲染页面抓取
- 网页截图（整页或指定区域）
- PDF 生成
- 控制台日志捕获

### 独特的终端渲染

> Carbonyl 支持将页面渲染为 ASCII 字符画，直接在终端查看效果：

```bash
carbonyl --ascii https://example.com
```

> 这在快速预览页面而不离开终端时非常有用。

---

## :package: 安装方式

```bash
# npm 全局安装
npm install -g carbonyl

# 或下载对应平台的二进制
curl -fsSL https://github.com/browser-automation/carbonyl/releases/latest/download/carbonyl-linux.tar.gz | tar xz
```

---

## :keyboard: 常用命令

### 基本使用

```bash
# 截图
carbonyl --screenshot https://example.com output.png

# 生成 PDF
carbonyl --pdf https://example.com output.pdf

# 终端 ASCII 预览
carbonyl --ascii https://example.com

# 交互模式
carbonyl https://example.com
```

### 进阶用法

```bash
# 指定视口大小截图
carbonyl --screenshot --viewport-width=1920 --viewport-height=1080 https://example.com output.png

# 等待页面加载完成
carbonyl --wait-until=networkidle2 --screenshot https://example.com output.png

# 隐藏元素
carbonyl --hide-element=".advertisement" --screenshot https://example.com output.png
```

### 编程调用

```javascript
import { Chromium } from 'carbonyl';

const browser = await Chromium.launch();
const page = await browser.newPage();

await page.goto('https://example.com');
await page.screenshot({ path: 'output.png' });

await browser.close();
```

---

## :bulb: 适用场景

### :white_check_mark: 适合的场景

| 场景 | 说明 |
|------|------|
| 服务端截图服务 | 快速搭建截图 API，不依赖 Puppeteer 的 Node.js 环境 |
| CI/CD 自动化测试 | 轻量级浏览器环境，GitHub Actions 等场景 |
| 网页内容抓取 | 需要 JS 渲染的 SPA 应用 |
| PDF 生成 | 服务端批量生成报告 |
| 终端快速预览 | 查看网页而不离开终端 |

### :x: 不适合的场景

- 需要完整浏览器交互（点击、表单提交）
- 复杂的多页面自动化流程
- 需要 Chrome 扩展支持

---

## :scales: 对比其他工具

| 特性 | Carbonyl | Puppeteer | Playwright |
|------|:--------:|:---------:|:----------:|
| 启动速度 | :rocket: 快 | :snail: 中等 | :snail: 中等 |
| 语言生态 | 多语言 | Node.js | 多语言 |
| 功能完整度 | 基础为主 | 完整 | 最完整 |
| 终端渲染 | :white_check_mark: | :x: | :x: |
| 安装便捷性 | :star::star::star: | :star::star: | :star::star: |

---

## :wrench: 实际应用示例

### 快速搭建截图 API

```bash
# 简单脚本
#!/bin/bash
url=$1
output=$2
carbonyl --screenshot --viewport-width=1280 "$url" "$output"
```

> 配合 `nc` 或简单的 HTTP 服务器可以快速做成截图服务。

### 批量生成 PDF 报告

```bash
# urls.txt 每行一个 URL
while read url; do
  name=$(basename "$url")
  carbonyl --pdf "$url" "reports/${name}.pdf"
done < urls.txt
```

---

## :warning: 注意事项

> - Carbonyl 仍处于活跃开发中，API 可能会有变化
> - 部分复杂的 WebGL 特性可能不支持
> - 调试能力相对简单，建议先用交互模式测试

---

## :link: 相关工具

- [[01-tunnelto]] — 本地服务公网暴露
- [[02-zoxide]] — 智能目录跳转
