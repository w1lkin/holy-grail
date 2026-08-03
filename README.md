# 掷筊问卜（Holy Grail）

纯前端趣味占卜游戏：掷出圣筊，获得趣味签文与建议。

## 特性

- **纯静态**：单文件 `index.html`（内联 CSS/JS），零依赖、无构建步骤。
- **无需联网**：占卜逻辑全部在浏览器本地运行。
- **数据本地**：无账号、无后端，结果即时生成。
- **移动端适配**：针对触屏与微信 webview 优化。
- **分享卡片**：游戏内可生成 1200×1600 分享图（二维码需联网）。

## 本地运行

```sh
cd holy-grail
python3 -m http.server 8000
# 浏览器打开 http://localhost:8000
```

> 分享卡片的二维码依赖 `api.qrserver.com`，必须经 `http(s)` 来源加载，请用本地服务器方式打开，不要直接 `file://` 打开。

## 文件结构

```
holy-grail/
└── index.html   # 单文件，含全部 HTML/CSS/JS
```

## 部署

已部署至 Cloudflare Pages：`holy-grail-b8u.pages.dev`
