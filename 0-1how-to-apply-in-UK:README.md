# 英国硕士 DIY 选校导航

从零开始，用免费网页工具完成英国硕士选校和申请。

不需要中介，不需要付费工具。按页面内 SOP 一步步操作即可。

---

## 目录

- [在线体验](#在线体验)
- [覆盖内容](#覆盖内容)
- [内置功能](#内置功能)
- [使用方式](#使用方式)
- [文件结构](#文件结构)
- [如何自定义](#如何自定义)
- [部署到 GitHub Pages](#部署到-github-pages)
- [常见问题](#常见问题)
- [已知限制](#已知限制)
- [许可证](#许可证)

---

## 在线体验

https://你的用户名.github.io/uk-master-guide/0-1%20how%20to%20apply%20master%20in%20UK.html

> 建议先把文件名改成 `apply.html`，避免 URL 里出现空格编码。

---

## 覆盖内容

| 步骤 | 内容 |
|------|------|
| Step 1 | 背景评估（Apply UK Center / UNILINK / DeepSeek） |
| Step 2 | 查询认可名单（List） |
| Step 3 | 查学费与核心课程（Whatuni / UniPath / 大学官网） |
| Step 4 | 官网信息核实四步法 |
| Step 5 | 文书写作（CV / PS / RL） |
| Step 6 | 确认截止时间，开始申请 |
| 附录 1 | 留学常用缩写速查表（可打印 / 保存 PDF） |
| 附录 2 | 视频引导教程 |
| 附录 3 | 链接失效急救站（404 怎么办） |

### 重点模块

- **List 制度详解**：英国名校的"准入制"逻辑，附通用查询 SOP
- **卡不卡背景判断 SOP**：7 步判断专业是否接受你的本科背景
- **核心课程查询 SOP**：6 步找到 Core Modules 的详细内容
- **文书写作 Prompt**：一键复制发给 DeepSeek，多轮追问产出 CV + 2 PS + 2 RL

---

## 内置功能

### 📋 Prompt 一键复制

- 所有 Prompt 框右上角自动出现"复制"按钮
- 点击后复制到剪贴板
- 支持 `navigator.clipboard`，失败时降级到 `execCommand`
- 复制成功显示"✅ 已复制"，1.8 秒后恢复

### 🔍 页面内搜索

| 操作 | 效果 |
|------|------|
| 输入关键词 | 300ms 防抖后高亮所有命中 |
| 回车 | 跳到下一个命中并滚动到屏幕中央 |
| Esc | 清空搜索，还原页面 |
| 点 ✕ | 同上 |
| 右上角计数 | 显示"3 处"或"2 / 5" |

### 🌙 暗色模式切换

| 行为 | 效果 |
|------|------|
| 首次打开 | 跟随系统 `prefers-color-scheme` |
| 点击右下角按钮 | 手动切换，存入 `localStorage` |
| 再次打开 | 读取上次选择 |

---

## 使用方式

1. 双击用浏览器打开（推荐 Chrome / Edge / Safari）
2. 顶部搜索框输入关键词（如 "List"、"PS"）→ 回车跳转
3. 找到 Prompt 框 → 点右上角"📋 复制"→ 粘贴到 DeepSeek
4. 右下角 🌙 切换暗色模式
5. 按页面内 SOP 一步步操作

---

## 文件结构

```
.
├── 0-1 how to apply master in UK.html   # 页面 A
└── README_apply.md                      # 本文档
```

如果想部署到 GitHub Pages，建议把文件名改为 `apply.html`，避免 URL 里的空格编码。

---

## 如何自定义

### 修改内容

所有内容都是纯 HTML，直接搜索关键词修改即可。

| 想改什么 | 搜索什么 |
|----------|----------|
| 大学链接 | `uni-item` |
| SOP 步骤 | `sop-step` |
| Prompt 模板 | `prompt-text` |
| 缩写表 | `abbr-table` |
| 主题色 | `#2563eb`（蓝色主色） |
| 暗色样式 | `body.dark` |

### 新增一所大学

在 `.uni-grid` 里加一行：

```html
<a class="uni-item" href="https://官网链接" target="_blank">
  <span class="uni-name">大学名称 <span class="uni-rank">QS 排名</span></span>
  <span class="arrow">→</span>
</a>
```

### 修改主题色

页面主色是蓝色 `#2563eb`，全局替换即可：

```bash
# macOS
sed -i '' 's/#2563eb/#7c3aed/g' "0-1 how to apply master in UK.html"

# Linux
sed -i 's/#2563eb/#7c3aed/g' "0-1 how to apply master in UK.html"
```

### 关闭某个功能

| 想关掉 | 操作 |
|--------|------|
| 搜索框 | 删除 `<div class="search-box">` 和对应 JS |
| 暗色按钮 | 删除 `<button class="theme-toggle">` 和对应 JS |
| 复制按钮 | 删除 `initCopyButtons()` 整个 IIFE |

---

## 部署到 GitHub Pages

### 1. 文件准备

```
uk-master-guide/
├── index.html                          # 入口页（可选）
├── apply.html                          # 页面 A（原文件名建议改）
└── README_apply.md
```

### 2. 上传到 GitHub

1. 新建仓库 `uk-master-guide`（Public）
2. `Add file` → `Upload files` → 拖入所有文件
3. `Commit changes`

### 3. 开启 Pages

1. 仓库 → `Settings` → `Pages`
2. Source 选 `Deploy from a branch`
3. Branch 选 `main`，目录选 `/ (root)`
4. 点 `Save`，等 1~2 分钟
5. 访问 `https://你的用户名.github.io/uk-master-guide/apply.html`

### 4. 后续更新

```bash
git add .
git commit -m "update"
git push
```

Pages 会自动重新部署。

---

## 常见问题

### Q1：页面打开是白屏 / 样式全丢

- 确认文件保存为 `.html`，不是 `.txt`
- 确认用浏览器打开，不是文本编辑器
- 确认文件名里没有中文乱码

### Q2：复制按钮点了没反应

- 部分浏览器（尤其是非 HTTPS 环境）会禁用 `navigator.clipboard`
- 页面已内置降级方案（`execCommand`），会自动 fallback
- 如果还是不行，手动选中 Prompt 文本复制即可

### Q3：搜索不到某些词

- 搜索只处理**文本节点**，不处理图片、链接地址
- 输入关键词后会自动跳到第一个命中
- 如果显示"无结果"，说明页面里确实没有这个词

### Q4：暗色模式下某些文字看不清

- 暗色样式已覆盖所有主要卡片和表格
- 如果有遗漏，在 `body.dark` 段里加对应选择器即可

### Q5：链接失效怎么办

- 页面内置了 **「链接失效急救站」**
- 按 SOP 操作：回官网首页 → 搜索框 → Google `site:` 搜索 → 联系学校

### Q6：能改成英文版吗

- 可以，全文替换中文文案即可
- 但页面里的 `Ctrl+F`、`site:` 等操作说明建议保留中英对照

---

## 已知限制

- 所有链接都指向第三方官网，链接失效无法自动修复
- 不保存任何用户数据（localStorage 只存暗色偏好）
- 无错误边界，如果某个链接挂了，页面本身不会崩
- 暗色模式为手动实现，不是系统级主题切换

---

## 后续可扩展

| 优先级 | 功能 | 说明 |
|--------|------|------|
| P0 | 更多大学 | 覆盖 QS 前 300 |
| P0 | 中英双语 | 方便英文申请者 |
| P1 | 时间线计算器 | 输入入学年份，自动生成 DDL |
| P1 | 预算计算器 | 输入专业和城市，估算总花费 |
| P1 | 收藏功能 | 把感兴趣的 SOP 收藏到本地 |
| P2 | 进度追踪 | 勾选已完成的步骤 |
| P2 | 导出 PDF | 把整个页面导出为 PDF |
| P3 | 接入 AI | 用 API 替换硬编码 Prompt |

---

## 许可证

MIT