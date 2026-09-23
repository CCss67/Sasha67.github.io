# Changelog

本项目的所有重要变更都会记录在此文件。

格式参考 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.0.0/)，
版本号遵循 [语义化版本](https://semver.org/lang/zh-CN/)。

---

## [Unreleased]

### 计划中

- v1.1：界面调整环节时长
- v1.2：导出 `.pptx` 文件
- v1.3：导出 Markdown / Word
- v1.4：保存历史教案
- v1.5：教案模板自定义
- v2.0：接入 LLM 智能生成

---

## [1.0.0] - 2026-09-16

### 新增

- 🎯 **三年龄段教案生成**
  - 小班（3-4 岁）：模仿发音，初步感知
  - 中班（4-5 岁）：清晰说出，理解句型
  - 大班（5-6 岁）：自信表达，情境运用

- 📝 **完整教案结构**
  - 活动目标（3 条 + 发展水平说明）
  - 9 个活动环节（引入 / Chant / Circle / 闪卡 / 3 个游戏 / 总结 / 再见）
  - 时长分配与自动总时长计算
  - 课后反思引导（5 条）

- 📊 **PPT 幻灯片模式**
  - 全屏幻灯片展示
  - 封面 + 步骤页 + 总结页 + 全英文稿件页
  - 键盘控制（← → 空格 Esc）
  - 全屏切换
  - 导出为 PDF（浏览器打印）

- 📄 **全英文教师稿件**
  - 可照读脚本
  - 一键复制到剪贴板

- 🎨 **交互功能**
  - 年龄段筛选 Tab（全部 / 小班 / 中班 / 大班）
  - 环节编辑（增删）
  - 总时长自动计算
  - 移动端适配

### 技术说明

- 纯前端、零依赖、单文件
- 使用**模板引擎**驱动生成，非 AI 智能生成
- 所有数据在浏览器本地处理，不上传服务器

### 已知限制

- 输入不同单词只替换单词部分，教案结构不变
- 不能导出 `.pptx` 文件
- 不能保存历史教案
- 环节时长只能通过代码修改

---

## v2.0 迭代方向

> 本章节明确 v2.0 的核心目标：**从模板引擎升级为 LLM 智能生成。**

### 背景

当前版本（v1.0）使用**模板引擎**生成教案：

- 所有教案来自预设模板
- 输入不同单词只替换单词部分
- 教案结构、句式、活动名称完全一致

这带来三个问题：

1. **重复感**：老师用几次就会发现生成的教案大同小异
2. **缺乏针对性**：不能根据具体单词（如 `elephant` vs `apple`）匹配更合适的游戏
3. **稿件生硬**：全英文稿件是固定模板拼接，不够自然

v2.0 的核心目标就是解决这三个问题。

### 核心变化

#### 1. 接入 LLM API

**支持的服务商**：

| 服务商 | 模型 | 特点 |
|--------|------|------|
| OpenAI | GPT-4o / GPT-4o-mini | 质量最高，需付费 |
| DeepSeek | deepseek-chat | 中文友好，性价比高 |
| 通义千问 | qwen-max | 国内访问快 |
| 智谱 | glm-4 | 免费额度 |

**接入方式**：

```js
// 用户可自填 API Key，存 localStorage
const config = {
  mode: 'auto',  // 'template' | 'llm' | 'auto'
  provider: 'deepseek',
  apiKey: 'sk-xxx',
  model: 'deepseek-chat'
};
```

#### 2. Prompt 工程

**输入结构**：

```js
{
  words: ['apple', 'banana', 'cat', 'dog'],
  grammar: 'I like ...',
  age: 'small',           // 年龄段
  style: '游戏化',         // 教学风格
  duration: 30,           // 目标时长（分钟）
  theme: '水果主题'        // 可选主题
}
```

**输出结构**（JSON）：

```json
{
  "objectives": ["目标1", "目标2", "目标3"],
  "steps": [
    {
      "name": "引入",
      "desc": "教师用...",
      "time": 3,
      "materials": ["玩偶", "闪卡"],
      "teacherScript": "Hello, everyone!..."
    }
  ],
  "totalTime": 30,
  "reflection": ["反思问题1", "反思问题2"]
}
```

**Prompt 模板**（示例）：

```
你是一位资深幼儿园英语教师，请为 [年龄段] 幼儿设计一节 [时长] 分钟的英语活动课。

核心单词：[单词列表]
核心句型：[句型]
教学风格：[风格]

要求：
1. 设计 8-10 个活动环节，包含引入、练习、游戏、总结
2. 每个环节给出具体的教师指导语和活动描述
3. 游戏设计要考虑 [年龄段] 幼儿的认知特点
4. 语言难度要与 [年龄段] 匹配
5. 输出 JSON 格式，包含 objectives / steps / totalTime / reflection

请开始设计。
```

#### 3. 生成质量提升

| 维度 | v1.0 模板引擎 | v2.0 LLM 生成 |
|------|---------------|---------------|
| 内容重复度 | 每次都一样 | 每次不同 |
| 单词针对性 | 无 | 根据单词匹配游戏 |
| 语言自然度 | 模板拼接 | 自然生成 |
| 年龄适配 | 固定模板 | 动态调整难度 |
| 教学风格 | 固定 | 可选（游戏化/学术化/情境化）|
| 时长控制 | 固定 | 可自定义 |

#### 4. 降级方案

**核心原则**：AI 不是必须的，离线永远可用。

```js
async function generateLesson(input) {
  // 1. 如果用户选择 template 模式，直接用模板
  if (config.mode === 'template') {
    return generateByTemplate(input);
  }
  
  // 2. 如果选择 llm 或 auto，尝试调用 API
  if (config.apiKey) {
    try {
      const result = await generateByLLM(input);
      return result;
    } catch (error) {
      console.warn('LLM 生成失败，降级为模板引擎', error);
      // 3. 失败自动降级
      return generateByTemplate(input);
    }
  }
  
  // 4. 没有 API Key，降级为模板
  return generateByTemplate(input);
}
```

#### 5. 架构调整

**现有函数**：

```js
function generateLesson(age, words, grammar) {
  // 模板引擎逻辑
}
```

**v2.0 拆分**：

```js
// 模板引擎（保留）
function generateByTemplate(input) { ... }

// LLM 生成（新增）
async function generateByLLM(input) { ... }

// 统一入口
async function generateLesson(input) {
  // 根据 config.mode 和 apiKey 决定走哪条路
}

// Prompt 构建器（新增）
function buildPrompt(input) { ... }

// 响应解析器（新增）
function parseLLMResponse(response) { ... }
```

### UI 变化

v2.0 计划在输入区新增：

```
┌─────────────────────────────────────────────┐
│ 生成模式：  ○ 模板引擎  ● AI 智能  ○ 自动    │
│                                             │
│ API 配置（可选）：                           │
│   服务商：[DeepSeek ▼]                       │
│   API Key：[________________]  [测试连接]    │
│                                             │
│ 教学风格：  ○ 游戏化  ○ 学术化  ○ 情境化     │
│ 目标时长：  [30] 分钟                        │
└─────────────────────────────────────────────┘
```

### 兼容性

- **向后兼容**：不配置 API Key 时，行为与 v1.0 完全一致
- **数据兼容**：教案数据结构保持不变，仅在 step 里新增可选字段
- **UI 兼容**：默认折叠 API 配置，不干扰现有用户

### 开发计划

| 阶段 | 任务 | 预计工期 |
|------|------|----------|
| 1 | 抽象 `generateLesson` 为统一入口 | 0.5 天 |
| 2 | 实现 `buildPrompt` 和 `parseLLMResponse` | 1 天 |
| 3 | 接入 DeepSeek API（最简单）| 0.5 天 |
| 4 | 加 API 配置 UI | 1 天 |
| 5 | 加降级逻辑和错误处理 | 0.5 天 |
| 6 | 测试各服务商兼容性 | 1 天 |
| 7 | 文档和示例 | 0.5 天 |
| **合计** | | **约 5 天** |

### 风险与挑战

| 风险 | 应对 |
|------|------|
| API 响应格式不稳定 | Prompt 强制 JSON 输出 + 解析容错 |
| API 费用 | 默认用 DeepSeek 等低价服务 |
| 用户 API Key 泄露 | 只存本地，不上传；提示用户风险 |
| 生成内容不符合教学规范 | Prompt 里加入教学法约束 |
| 生成速度慢 | 加 loading 动画 + 流式输出 |
| 网络问题 | 自动降级到模板引擎 |

### 里程碑

- [ ] v2.0-alpha：接入 DeepSeek，命令行测试通过
- [ ] v2.0-beta：加 UI 配置，内测
- [ ] v2.0-rc：多服务商支持，公测
- [ ] v2.0：正式发布

---

## 版本历史

| 版本 | 日期 | 说明 |
|------|------|------|
| 1.0.0 | 2026-09-16 | 首次发布，模板引擎驱动 |
| 2.0.0 | 规划中 | 接入 LLM，实现智能生成 |

---

## 许可证

MIT