# 掷筊问卜（Holy Grail）

纯前端单机趣味占卜：掷出圣筊，给出趣味签文与建议。

## 单机版特性

- **纯静态**：仅 `index.html`（内联 CSS/JS），零依赖、无构建步骤。
- **无需联网**：占卜逻辑全部在浏览器本地运行。
- **数据本地**：无账号、无后端，结果即时生成。
- **即开即玩**：双击 `index.html` 即可运行。

## 本地运行

```sh
cd holy-grail
python3 -m http.server 8000
# 浏览器打开 http://localhost:8000
```

或直接用浏览器打开 `index.html`。

## 文件结构

```
holy-grail/
└── index.html   # 单文件，含全部 HTML/CSS/JS
```

## 部署

可一键部署到 Cloudflare Pages（根目录即 `index.html`）。

## 版本

当前分支：`release/1.0.0`
