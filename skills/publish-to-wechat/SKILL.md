---
name: "publish-to-wechat"
description: "AI-ready skill to format and publish Markdown articles to WeChat Official Accounts using Wenyan CLI."
---

# Wenyan CLI 发布工具 (WeChat Publisher)

这是一个专门为 AI Agent 设计的技能，用于将标准的 Markdown 文档转换为符合微信公众号排版要求的富文本并直接发布。它集成了自动化样式注入、代码高亮处理以及素材库图片自动上传功能。

## 核心能力

- **自动化排版**：支持多种内置主题（如 `orangeheart`）和代码高亮方案。
- **智能素材处理**：自动解析 Markdown 中的本地或网络图片，并上传至微信素材库。
- **元数据驱动**：通过 YAML Frontmatter 自动配置文章标题、封面、作者和原文链接。
- **高度可定制**：支持自定义 CSS 主题注入，满足个性化品牌视觉。
- **灵活输入**：支持本地文件、Web URL 或直接传入 Markdown 字符串。

## 安装与配置

该工具基于 Node.js 运行，请确保环境已配置。

```bash
npm install -g wenyan-md-cli-evlon
```

### 身份验证
Agent 必须确保已配置微信公众号凭证。可以通过以下两种方式之一提供：
1. **环境变量**（推荐用于自动化流程）：
   - `WECHAT_APP_ID`: 微信公众号 AppID
   - `WECHAT_APP_SECRET`: 微信公众号 AppSecret
2. **命令行参数**：
   - `--app-id <id>`
   - `--app-secret <secret>`

## AI Agent 指令指南

### 什么时候调用此技能
当用户要求"发布到微信"、"同步到公众号"或"排版 Markdown 准备发布"时调用。

### Frontmatter 约束 (必须包含)
文章开头必须包含以下 YAML 块，否则发布接口将返回错误：

```yaml
---
title: 文章标题
cover: ./cover.jpg # 若缺省则自动取正文第一张图
author: 作者名称
source_url: https://example.com/original-article # 可选，原文链接
---
```

### 参数说明
- `input-content`: **(可选)** 直接传入 Markdown 内容字符串。
- `-f, --file <path>`: **(可选)** 从本地文件或 Web URL 读取 Markdown 内容。
- `-t, --theme <theme-id>`: 排版主题 ID（默认 `default`）。
- `-h, --highlight <highlight-theme-id>`: 代码高亮主题 ID（默认 `solarized-light`）。
- `-c, --custom-theme <path>`: 自定义主题 CSS 文件路径。
- `--mac-style`: 启用 Mac 风格代码块（默认 `true`）。
- `--no-mac-style`: 禁用 Mac 风格代码块。
- `--footnote`: 将链接转换为脚注（默认 `true`）。
- `--no-footnote`: 禁用脚注转换。
- `--server <url>`: 指定远程发布服务器 URL（SaaS 模式）。
- `--api-key <apiKey>`: 远程服务器的 API Key。
- `--app-id [id]`: 指定微信公众号 AppID（优先级高于环境变量）。
- `--app-secret [secret]`: 指定微信公众号 AppSecret（优先级高于环境变量）。

## 常用操作示例

### 1. 标准发布（使用环境变量认证）
```bash
wenyan publish -f my-article.md
```

### 2. 指定主题与高亮发布
```bash
wenyan publish -f article.md -t orangeheart -h solarized-light
```

### 3. 使用命令行参数指定凭证
```bash
wenyan publish -f article.md --app-id wx123456 --app-secret secret123
```

### 4. 使用自定义 CSS 主题
```bash
wenyan publish -f article.md -c ./my-custom-theme.css
```

### 5. 禁用 Mac 风格代码块和脚注
```bash
wenyan publish -f article.md --no-mac-style --no-footnote
```

### 6. 主题管理（列出可用主题）
```bash
wenyan theme -l
```

## 故障排除 (Agent 专用)

- **Error: invalid appid/secret**: 检查 `--app-id`/`--app-secret` 参数或环境变量 `WECHAT_APP_ID`/`WECHAT_APP_SECRET` 是否正确。
- **图片上传失败**: 
  - 检查 Markdown 中的图片路径。如果是本地路径，Agent 需确认文件在当前工作目录中真实存在。
  - 确保图片格式受微信支持（jpg, png, gif 等）。
- **样式冲突**: 如果用户反馈排版错乱，建议尝试禁用 Mac 风格代码块：`--no-mac-style`。
- **Network Error**: 如果使用 `--server`，请检查网络连接及 `--api-key` 是否正确。

## 关联项目
- **GitHub**: [caol64/wenyan-cli](https://github.com/caol64/wenyan-cli)
- **核心逻辑**: 基于 `@wenyan-md/core` 渲染引擎。

## 许可证
MIT
