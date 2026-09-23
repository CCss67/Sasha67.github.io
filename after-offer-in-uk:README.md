# 英国硕士 Offer 后保姆级指南

从拿到 Con Offer 到登机出发，每一步都有免费官方链接和操作 SOP。

按页面内 7 步流程，自己搞定全部手续。

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

https://你的用户名.github.io/uk-master-guide/after%20offer%20in%20UK.html

> 建议先把文件名改成 `after-offer.html`，避免 URL 里出现空格编码。

---

## 覆盖内容

| 步骤 | 内容 |
|------|------|
| Step 0 | 全程时间线总览 |
| Step 1 | Con Offer 换 Unconditional Offer |
| Step 2 | 提交语言成绩 |
| Step 3 | 申请语言班（Pre-sessional English） |
| Step 4 | 接受 Uncon 并换取 CAS |
| Step 5 | 申请学生签证（Student Visa） |
| Step 6 | 申请学校宿舍 |
| Step 7 | 订机票 & 行前准备 |
| 附录 1 | 链接失效急救站（404 怎么办） |
| 附录 2 | 语言班链接专项查找 SOP |
| 附录 3 | 常用官网关键词中英对照 |

### 重点模块

- **时间线总览**：3 月到 9 月的完整节奏
- **语言班链接专项 SOP**：语言班页面每年变动，附专项查找方法
- **CAS 换发 SOP**：接受 Uncon → 缴押金 → 核对 CAS Draft → 收正式 CAS
- **签证存款证明避坑**：28 天连续存款 + 31 天有效期
- **前 200 名大学链接**：语言班 / CAS / 宿舍三个板块，覆盖 QS 前 200

---

## 内置功能

### 📌 悬浮目录快速跳转

- 页面顶部 sticky 目录，横向滚动
- 点击任意标签跳到对应步骤
- 与搜索框错开，互不遮挡

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
2. 顶部悬浮目录快速跳转
3. 顶部搜索框输入关键词（如 "CAS"、"语言班"）→ 回车跳转
4. 找到 Prompt 框 → 点右上角"📋 复制"→ 粘贴到邮件或 DeepSeek
5. 右下角 🌙 切换暗色模式
6. 按页面内 SOP 一步步操作

---

## 文件结构

```
.
├── after offer in UK.html   # 页面 B
└── README_after_offer.md    # 本文档
```

如果想部署到 GitHub Pages，建议把文件名改为 `after-offer.html`，避免 URL 里的空格编码。

---

## 如何自定义

### 修改内容

所有内容都是纯 HTML，直接搜索关键词修改即可。

| 想改什么 | 搜索什么 |
|----------|----------|
| 大学链接 | `uni-link` |
| SOP 步骤 | `sop-step` |
| 时间线 | `timeline-box` |
| 警告框 | `warning-box` |
| 悬浮目录 | `toc-item` |
| 主题色 | `#10b981`（绿色主色） |
| 暗色样式 | `body.dark` |

### 新增一所大学

在 `.uni-links-grid` 里加一行：

```html
<a class="uni-link" href="https://官网链接" target="_blank">
  <span class="rank">QS 排名</span>
  <span class="name">大学名称</span>
</a>
```

三个板块（语言班 / CAS / 宿舍）都要加。

### 修改主题色

页面主色是绿色 `#10b981`，全局替换即可：

```bash
# macOS
sed -i '' 's/#10b981/#f97316/g' "after offer in UK.html"

# Linux
sed -i 's/#10b981/#f97316/g' "after offer in UK.html"
```

### 修改悬浮目录

在 `.toc-scroll` 里增删 `.toc-item`：

```html
<a class="toc-item green" href="#step1">1 Con换Uncon</a>
```

颜色类名：`green` / `orange` / `blue` / `red`。

### 关闭某个功能

| 想关掉 | 操作 |
|--------|------|
| 搜索框 | 删除 `<div class="search-box">` 和对应 JS |
| 暗色按钮 | 删除 `<button class="theme-toggle">` 和对应 JS |
| 复制按钮 | 删除 `initCopyButtons()` 整个 IIFE |
| 悬浮目录 | 删除 `<div class="toc">` 整块 |

---

## 部署到 GitHub Pages

### 1. 文件准备

```
uk-master-guide/
├── index.html              # 入口页（可选）
├── after-offer.html        # 页面 B（原文件名建议改）
└── README_after_offer.md
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
5. 访问 `https://你的用户名.github.io/uk-master-guide/after-offer.html`

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

### Q2：悬浮目录被搜索框遮住

- 页面 B 的搜索框 `top: 0`，悬浮目录 `top: 56px`
- 如果你改了搜索框高度，需要同步改目录的 `top` 值

### Q3：复制按钮点了没反应

- 部分浏览器（尤其是非 HTTPS 环境）会禁用 `navigator.clipboard`
- 页面已内置降级方案（`execCommand`），会自动 fallback
- 如果还是不行，手动选中文本复制即可

### Q4：搜索不到某些词

- 搜索只处理**文本节点**，不处理图片、链接地址
- 输入关键词后会自动跳到第一个命中
- 如果显示"无结果"，说明页面里确实没有这个词

### Q5：暗色模式下某些文字看不清

- 暗色样式已覆盖所有主要卡片、表格、SOP 框
- 如果有遗漏，在 `body.dark` 段里加对应选择器即可

### Q6：链接失效怎么办

- 页面内置了 **「链接失效急救站」**
- 按 SOP 操作：回官网首页 → 搜索框 → Google `site:` 搜索 → 联系学校
- 语言班链接最容易失效，页面底部有专项查找 SOP

### Q7：语言班链接为什么总是打不开

- 语言班由各大学的**语言中心**独立管理
- 每年调整课程周数、开课时间、申请截止日期
- 页面路径几乎每年变动，链接失效是**正常现象**
- 不代表学校取消了语言班，按专项 SOP 查找即可

---

## 已知限制

- 所有链接都指向第三方官网，链接失效无法自动修复
- 不保存任何用户数据（localStorage 只存暗色偏好）
- 无错误边界，如果某个链接挂了，页面本身不会崩
- 暗色模式为手动实现，不是系统级主题切换
- 签证、CAS、存款证明政策每年可能微调，务必以官网为准

---

## 后续可扩展

| 优先级 | 功能 | 说明 |
|--------|------|------|
| P0 | 更多大学 | 覆盖 QS 前 300 |
| P0 | 中英双语 | 方便英文申请者 |
| P1 | DDL 倒计时 | 输入入学年份，自动倒计时 |
| P1 | 存款计算器 | 输入学费 + 城市，算资金证明金额 |
| P1 | 进度追踪 | 勾选已完成的步骤 |
| P2 | 邮件模板库 | 招生办 / 语言中心 / 宿舍办邮件模板 |
| P2 | 导出 PDF | 把整个页面导出为 PDF |
| P3 | 接入 AI | 用 API 自动回答申请问题 |

---

## 许可证

MIT