# SubFrame - 图片字幕编辑器

## 项目概述

面向短视频创作者、自媒体运营者、梗图制作者的轻量级网页图片字幕编辑器。用户上传图片后，可以快速添加多行字幕（或让 AI 生成），实时预览分段字幕条效果，一键导出为高清 PNG。

单 HTML 文件，无第三方依赖，浏览器直接打开即用，所有数据仅在本地处理。

## 文件结构

```
网页-图片字幕生成器/
├── index.html   # 唯一源文件，包含全部 HTML / CSS / JS（约 950 行）
├── prd.md       # 产品需求文档（完整 PRD，含 MVP 范围与版本规划）
├── README.md    # GitHub 仓库说明
└── CLAUDE.md    # 本文件，Claude Code 项目指引
```

## 技术架构

### 渲染引擎
- HTML5 Canvas 2D API
- 绘制顺序：原图 → 字幕条背景 → 文字描边（strokeText）→ 文字填充（fillText）
- 描边必须先于填充，否则描边会覆盖填充文字

### 状态管理
全局 `state` 对象，五个模块：
- `image` — 图片 src（dataURL）、宽高、文件名
- `subtitles` — 字幕数组，每项 `{id, text, visible}`
- `style` — 字体大小/颜色/粗细、描边颜色/粗细、对齐方式
- `layout` — 布局模式、字幕条高度/颜色/透明度/间距、位置、偏移
- `export` — 导出清晰度（scale 倍率）

### 4 种布局模式
1. `segmentedBar` — 分段字幕条：每条字幕独占一个半透明横向区域
2. `simpleBottom` — 普通底部：多行字幕直接叠放，无背景
3. `fullBox` — 整体字幕框：所有字幕在一个统一的半透明框内
4. `noBackground` — 无背景描边：仅白字黑描边，不显示字幕条

### 字幕位置计算（PRD 9.3 节）
```
整体高度 = N × barHeight + (N-1) × barGap
组顶部Y = H - bottomOffset - 整体高度
第 i 条 Y = 组顶部Y + i × (barHeight + barGap)
```

### AI 文案生成
- 调用 DeepSeek API（`deepseek-chat` 模型）
- 支持图片 vision 输入（base64 dataURL）
- API Key 存储在浏览器 localStorage，不会外传
- 生成 3-6 行短句，每行不超过 15 字

### 导出机制
- 创建离屏 Canvas，按原图尺寸 × scale 重新绘制
- 所有尺寸参数乘以 scale 系数
- `canvas.toBlob()` 生成 PNG 下载

### 持久化
- localStorage 存储编辑状态 JSON
- 图片 dataURL 超过 4MB 时不保存图片数据（仅保存配置参数）
- 页面刷新后恢复：字幕文本、样式参数、布局设置

## 设计风格

- **主题**: 电影级暗色工作室（Dark Studio）
- **强调色**: 琥珀金 `#f0a030`，渐变到 `#fbbf24`
- **背景**: 深炭色 `#0f0f11` → `#16161a` → `#1c1c22` 层次
- **字体**: DM Sans（英文/数字）+ Noto Sans SC（中文），回退 PingFang SC → Microsoft YaHei
- **预览区**: 暗色网格背景 + 径向渐变氛围光，画布带多层阴影

## 开发注意事项

- 所有代码在 `index.html` 单文件中，修改时注意保持 HTML / CSS / JS 三部分的结构清晰
- Canvas 渲染中所有尺寸参数需乘以 scale 系数：预览时 scale 为画布适配系数，导出时 scale 为 export.scale
- 字幕文字过长时自动缩小字体（`drawSubText` 函数中的 while 循环）
- 修改样式只改 CSS 部分，修改功能只改 JS 部分，避免互相影响
- 新增控件需同步更新 `syncUIFromState()` 和对应的 `updateStyle()` / `updateLayout()` 函数
- localStorage 有 5-10MB 限制，大图片的 dataURL 可能导致存储失败

## 版本规划（参考 prd.md）

- **MVP**（当前）— 图片上传 + 字幕输入 + Canvas 预览 + 字幕条布局 + PNG 导出
- **V1.1** — 样式模板、水印设置、拖动字幕组、JPG/WebP 导出、移动端优化
- **V1.2** — 单条字幕独立样式、批量上传、项目配置导入导出、平台尺寸预设
- **V2.0** — 视频截图导入、AI 识别字幕、OCR、模板市场、云端保存
