# SubFrame - 图片字幕编辑器

## 项目概述

面向短视频创作者的轻量级网页图片字幕编辑器。单 HTML 文件，无依赖，浏览器直接打开即用。

## 文件结构

- `index.html` — 唯一源文件，包含全部 HTML/CSS/JS
- `prd.md` — 产品需求文档
- `README.md` — GitHub 仓库说明

## 技术要点

- **渲染引擎**: HTML5 Canvas 2D API，绘制顺序为 原图 → 字幕条背景 → 文字描边 → 文字填充
- **状态管理**: 全局 `state` 对象，包含 image / subtitles / style / layout / export 五个模块
- **持久化**: localStorage 存储编辑状态（大图超过 4MB 时不保存图片数据）
- **AI 文案**: 调用 DeepSeek API（`deepseek-chat` 模型），支持图片 vision 输入
- **导出**: 离屏 Canvas 按原图尺寸 × scale 重新绘制，`canvas.toBlob()` 下载 PNG

## 设计风格

电影级暗色主题（Dark Studio），琥珀金（#f0a030）为强调色，DM Sans + Noto Sans SC 字体组合。

## 开发注意事项

- 修改样式只改 CSS 部分，不要改动 JS 逻辑
- Canvas 渲染中所有尺寸参数需乘以 scale 系数，导出时 scale 为 export.scale，预览时 scale 为适配系数
- 描边必须先于填充绘制（strokeText → fillText）
- 中文字体回退链：DM Sans → Noto Sans SC → PingFang SC → Microsoft YaHei
