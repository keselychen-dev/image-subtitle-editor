# SubFrame - 图片字幕编辑器

一个轻量级的网页图片字幕编辑器，专为短视频创作者、自媒体运营者和梗图制作者设计。上传图片，输入字幕（或让 AI 生成），实时预览，一键导出。

## 功能特性

- **图片上传** — 支持拖拽/点击上传，兼容 PNG / JPG / WebP，超大图自动缩放
- **多行字幕** — 每行一条字幕，支持一键清空
- **4 种布局模式** — 分段字幕条、普通底部、整体字幕框、无背景描边
- **文字样式** — 字体大小/颜色/粗细、描边颜色/粗细、对齐方式
- **字幕条设置** — 高度/颜色/透明度/间距
- **位置控制** — 顶部/居中/底部 + 偏移距离
- **AI 文案生成** — 接入 DeepSeek API，支持图片分析，根据场景自动生成字幕文案
- **高清导出** — 支持 1x / 2x / 3x 清晰度 PNG 导出
- **状态持久化** — 编辑内容自动保存到浏览器本地存储
- **暗色 UI** — 电影级暗色工作室风格，移动端响应式适配

## 快速开始

直接在浏览器中打开 `index.html` 即可使用，无需安装任何依赖或构建工具。

```bash
# 克隆仓库
git clone https://github.com/keselychen-dev/image-subtitle-editor.git

# 打开编辑器
open image-subtitle-editor/index.html
```

## AI 文案生成配置

1. 点击字幕输入区下方的「⚙ 设置 API Key」
2. 粘贴你的 [DeepSeek API Key](https://platform.deepseek.com/)
3. 输入主题后点击「AI 生成」，或上传图片后直接生成

API Key 仅保存在浏览器 localStorage 中，不会上传到任何服务器。

## 技术栈

- 纯 HTML / CSS / JavaScript 单文件
- HTML5 Canvas 2D 渲染
- DeepSeek API（文案生成）
- 无第三方依赖

## 许可证

MIT
