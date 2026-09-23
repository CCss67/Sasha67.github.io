# Companion · 情感支持人设系统

一个纯前端、单文件、零依赖的情感陪伴 Demo。

用户在首页发布一条动态，**10 秒内**会收到 3 位"真人"的回复。
每人设有独立的个人主页、年龄、城市、职业、兴趣标签。
用户看不到任何 AI 痕迹。

- 打开即用，无需注册，无需联网
- 单文件、离线可用、可部署到 GitHub Pages
- 数据存 localStorage，刷新不丢

---

## 目录

- [这是什么](#这是什么)
- [为什么做这个](#为什么做这个)
- [快速开始](#快速开始)
- [三位人设](#三位人设)
- [核心玩法](#核心玩法)
- [产品设计](#产品设计)
- [交互规则](#交互规则)
- [数据结构](#数据结构)
- [技术实现](#技术实现)
- [如何自定义](#如何自定义)
- [部署到 GitHub Pages](#部署到-github-pages)
- [常见问题](#常见问题)
- [已知限制](#已知限制)
- [后续规划](#后续规划)
- [许可证](#许可证)

---

## 这是什么

**Companion** 是一个 AI 情感陪伴 Demo。

它模拟真实交友软件的交互：用户发一条动态，会有"人"主动来找你聊天。

三个人设，各有自己的性格：

- **Luna** —— 温柔托底型，不追问，不分析
- **Kai** —— 话少心疼型，句句在点上
- **叶叶** —— 无条件的爱型，像妈妈一样唠叨

整个体验的目标是：**让用户感到"有人在乎我"，而不是"我在用一个 AI"。**

### 与传统 AI 聊天产品的差异

| 常见 AI 聊天 | Companion |
|--------------|-----------|
| 用户主动发起对话 | 人设主动来找你 |
| 明确告知是 AI | 全程像真人社交 |
| 单向问答 | 双向关系，可忽略、可回应 |
| 无记忆 | 记录聊天历史 |
| 无情感设计 | 情绪识别 + 加权回复 |

---

## 为什么做这个

### 起点：AI 陪伴产品的情感设计缺失

市面上大多数 AI 陪伴产品，功能齐全但**情感体验是空的**：

- 打开就要你说话，缺少"被想起"的感觉
- 回复速度固定，缺少"等待"的张力
- 回复内容模板化，缺少"被理解"的瞬间
- 一旦你不打开，就消失了

### 洞察：陪伴感来自"被想起"，不是"被响应"

用户访谈发现，真正让人感觉被陪伴的，是这些细节：

- **发完动态后有人主动回复** —— 像朋友圈，而不是像客服
- **回复有延迟** —— 像真的有人在另一个地方打字
- **不回复也不会消失** —— 你的注意力塑造你的信息流
- **未读不会积累成焦虑** —— 第二天醒来，世界是干净的

这些细节都不是"AI 能力"问题，而是"产品设计"问题。

### 产品假设

如果一个 AI 陪伴产品能做到这四点，用户会感觉"真的有人在乎我"：

1. **主动来找你** —— 发布后有人回复
2. **像真人一样慢** —— 回复有延迟、有打字感
3. **尊重你的注意力** —— 忽略的人减少出现，回应的人更多出现
4. **不制造焦虑** —— 未读次日清空

---

## 快速开始

1. 保存代码为 `index.html`
2. 双击用浏览器打开（推荐 Chrome / Edge / Safari）
3. 首页输入一句话 → 点"发布"
4. 10 秒内会陆续收到 3 条回复
5. 点回复进入聊天，或去"发现"看人设主页

不需要安装、不需要联网、不需要注册。

---

## 三位人设

| 人设 | 年龄 | 城市 | 职业 | 性格定位 | 回复风格 |
|------|------|------|------|----------|----------|
| **Luna** | 27 | 上海 | 心理咨询师 | 温柔托底 | 2~4 行，语气软，不追问 |
| **Kai** | 32 | 北京 | 创业者 | 话少心疼 | 1~3 行，句号多，命令式 |
| **叶叶** | 55 | 成都 | 退休教师 | 无条件的爱 | 3~5 行，直白，带唠叨 |

### 人设对象结构

```js
{
  key: 'luna',
  name: 'Luna',
  avatar: 'L',
  color: '#FF9A9E',
  cover: 'linear-gradient(...)',
  age: 27,
  city: '上海',
  job: '心理咨询师',
  bio: '一句话签名',
  tags: ['不追问', '温柔', ...],
  prompts: [
    { q: '最近让我放松的事', a: '下班后一个人散步。' }
  ],
  stats: { followers: 1280, likes: 3560 },
  replies: {
    tired: '...', sad: '...', happy: '...', anxious: '...', default: '...'
  },
  chatReplies: ['嗯嗯，我在听', ...]
}
```

### 每位人设的详细信息

**Luna · 27 · 上海 · 心理咨询师**

- 封面：粉色渐变
- 头像：L
- 签名：不追问，不分析。累了就歇一会儿，想说的时候我都在。
- 标签：不追问、温柔、轻陪伴、情绪稳定、话少
- 关于我：
  - 最近让我放松的事：下班后一个人散步，什么都不想。
  - 我希望遇到的你：不用完美，真实的就好。
  - 我的日常：养了一只叫"豆子"的猫。

**Kai · 32 · 北京 · 创业者**

- 封面：深色渐变
- 头像：K
- 签名：不多问，但句句在点上。你值得被好好对待。
- 标签：话少、直接、护短、行动派、有安全感
- 关于我：
  - 我的日常：早上六点跑步，晚上十点下班。
  - 我欣赏的品质：说到做到。
  - 周末去哪：山里，没信号的那种。

**叶叶 · 55 · 成都 · 退休教师**

- 封面：暖黄渐变
- 头像：叶
- 签名：不管你变成什么样，我都爱你。
- 标签：无条件的爱、直白、温暖、唠叨、心疼你
- 关于我：
  - 我的日常：早上买菜，下午跳广场舞。
  - 我最想说的：孩子，记得好好吃饭。
  - 我的愿望：希望你每天都开心。

---

## 核心玩法

### 1. 发布一条动态

首页顶部有一个输入框，写下想说的话，点「发布」。

发布后：

- 帖子出现在下方列表
- 会有 3 位人设主动来找你

### 2. 10 秒内收到 3 条回复

发布后，按权重挑选 3 位人设，依次回复：

| 顺序 | 到达时间 |
|------|----------|
| 第 1 条 | 1.0 ~ 3.0 秒 |
| 第 2 条 | 3.5 ~ 5.5 秒 |
| 第 3 条 | 6.0 ~ 8.0 秒 |

错峰到达，像真的有人陆续看到你。

### 3. 情绪识别

回复内容根据帖子的情绪关键词匹配：

| 情绪 | 触发词示例 |
|------|-----------|
| tired（累） | 累 / 疲惫 / 困 / 加班 / 熬夜 / 撑不住 |
| sad（难过） | 难过 / 伤心 / 哭 / 委屈 / 失落 / 崩溃 |
| happy（开心） | 开心 / 高兴 / 快乐 / 兴奋 / 太好了 / 棒 |
| anxious（焦虑） | 焦虑 / 紧张 / 压力 / 担心 / 害怕 / 慌 |
| default | 未命中任何关键词 |

### 4. 点回复进入聊天

点击任意回复卡片，进入与该人设的一对一聊天。

聊天里也可以继续对话，人设会根据你的消息内容继续回应。

### 5. 看人设主页

进「发现」页，可以看到三位人设的卡片。

点任意卡片，进入个人主页：

- 封面 + 头像
- 年龄、城市、职业
- 关注 / 获赞 / 年龄统计
- 兴趣标签
- 关于我问答
- 「和她聊天」按钮

### 6. 消息页

「消息」页聚合了三位人设的最后一条消息。

如果有未读，会显示红点和条数。

---

## 产品设计

### 核心设计决策

#### 决策 1：不暴露 AI 身份

| 方案 | 取舍 |
|------|------|
| ❌ 标注"AI 陪伴" | 用户会带着"测试 AI"的心态 |
| ✅ 伪装成真人 | 用户会带着"社交"的心态 |

体现：

- 人设有年龄、城市、职业
- 有"关注 / 获赞"数据
- 有"关于我"问答
- 没有一处提到 AI

#### 决策 2：10 秒内 3 条回复

| 方案 | 问题 |
|------|------|
| 1 条回复 | 像客服，不像社交 |
| 3 条同时到达 | 不真实，像群发 |
| ✅ 3 条错峰到达 | 像真的有人陆续看到你 |

#### 决策 3：权重系统

传统社交软件：你不回复，对方也不会消失。

本产品：**你的注意力会塑造你的信息流。**

- 忽略一个人 → 他出现得越来越少（×0.6）
- 回应一个人 → 他出现得越来越多（×1.2）
- 但不会完全消失（最低 0.1）

这不是 bug，是产品价值观：**关系是双向的**。

#### 决策 4：未读次日清空

传统软件：未读红点永久累积，制造焦虑。

本产品：**未读只属于"今天"。** 第二天醒来，世界是干净的。

与人设"陪伴"的定位一致：不制造压力，不索取注意力。

#### 决策 5：聊天气泡宽度 78%

| 宽度 | 效果 |
|------|------|
| 60% | 太窄，读起来累 |
| 70% | 偏窄 |
| ✅ 78% | 舒适，一行能放更多内容 |
| 85% | 太宽，缺少"对话感" |

配合 15px 字号 + 1.65 行高，长消息阅读体验最佳。

### 用户旅程

```
打开 App
   ↓
看到首页"分享点什么"
   ↓
发一条动态
   ↓
10 秒内陆续收到 3 条回复
   ↓
选择回应一个人 → 权重升高
选择忽略另一个人 → 权重降低
   ↓
次日打开 → 未读自动清空
   ↓
下次发布 → 出现的人设已经不同
```

### 设计原则

1. **不制造焦虑** —— 未读次日清空，没有催回复
2. **尊重注意力** —— 忽略的人会减少出现
3. **双向关系** —— 你的回应会让对方更常出现
4. **不暴露 AI** —— 全程像真人社交
5. **轻量优先** —— 单文件、零依赖、离线可用

---

## 交互规则

### 发布 → 回复

1. 用户发布 post
2. 系统根据 `weights` 权重，加权随机挑出 **3 位不重复**的人设
3. 3 条回复在 10 秒内依次到达
4. 每条回复初始为 `read: false`，并触发 `markUnread()`

### 权重系统

**设计目的**：你的注意力塑造你的信息流。

| 场景 | 权重变化 |
|------|----------|
| 初始值 | 1.0 |
| 用户没有打开某人的回复 | × 0.6 |
| 最低 | 0.1（不会完全消失） |
| 用户在聊天页主动回复某人 | × 1.2 |
| 最高 | 2.0 |

挑选算法（加权随机抽样）：

```js
function weightedPick(keys, count) {
  const pool = keys.map(k => ({ key: k, w: store.weights[k] || 1.0 }));
  const picked = [];
  for (let i = 0; i < count && pool.length; i++) {
    const total = pool.reduce((s, p) => s + p.w, 0);
    let r = Math.random() * total;
    let idx = 0;
    for (let j = 0; j < pool.length; j++) {
      r -= pool[j].w;
      if (r <= 0) { idx = j; break; }
    }
    picked.push(pool[idx].key);
    pool.splice(idx, 1);
  }
  return picked;
}
```

### 未读消息规则

**产生**：每条新回复到达时 `unread[charKey] += 1`，同时记录 `unreadDay = 今天日期`。

**清除**：

| 场景 | 行为 |
|------|------|
| 用户点开该回复卡片 | 清空该人设未读 |
| 用户进入该人设聊天页 | 清空该人设未读 |
| 当天不打开，次日启动 App | 所有未读自动清空 |

次日清空实现：

```js
function checkUnreadExpiry() {
  const today = todayStr();
  if (store.unreadDay && store.unreadDay !== today) {
    store.unread = {};
    store.unreadDay = null;
    save();
  }
}
```

每次 App 启动时调用一次。

### 状态流转图

```
用户发布 post
      ↓
按权重挑 3 人
      ↓
10 秒内 3 条回复依次到达
      ↓
产生未读 + 红点
      ↓
┌─────────────────────┬─────────────────────┐
│ 用户打开某条回复     │ 用户不打开           │
│      ↓              │      ↓              │
│ 该人设未读清零       │ 次日启动自动清空     │
│ 进入聊天页           │ 该人设权重 ×0.6     │
│ 回复 → 权重 ×1.2    │                     │
└─────────────────────┴─────────────────────┘
```

---

## 数据结构

### localStorage Key

```
companion_v2
```

### 顶层结构

```js
{
  posts: [],           // 所有帖子
  chats: {},           // 按人设分桶的聊天记录
  unread: {},          // 按人设分桶的未读数
  unreadDay: null,     // 未读产生日期（用于次日清空）
  weights: {},         // 按人设分桶的权重
  ignored: {},         // 按人设分桶的忽略次数
  currentChar: null    // 当前聊天对象
}
```

### posts 结构

```js
posts: [
  {
    id: '1730000000000_ab12',
    content: '今天好累',
    mood: 'tired',
    time: 1730000000000,
    replies: [
      {
        charKey: 'luna',
        text: '辛苦啦\n累的话就先歇一会儿...',
        time: 1730000001500,
        read: false
      }
    ]
  }
]
```

### chats 结构

```js
chats: {
  luna: [
    { role: 'bot', text: '我在呢...', time: 1730000000000 },
    { role: 'user', text: '今天好累', time: 1730000010000 },
    { role: 'bot', text: '辛苦啦...', time: 1730000015000 }
  ],
  kai: [...],
  yeye: [...]
}
```

### unread / weights / ignored 结构

```js
unread: { luna: 2, kai: 1 },
weights: { luna: 1.0, kai: 0.6, yeye: 1.44 },
ignored: { luna: 0, kai: 1, yeye: 0 }
```

### 持久化时机

| 操作 | 是否立即 save |
|------|---------------|
| 发布 post | ✅ |
| 新回复到达 | ✅ |
| 打开回复/聊天 | ✅ |
| 发送消息 | ✅ |
| 收到回复 | ✅ |
| 权重变化 | ✅ |

### 清空数据

浏览器控制台执行：

```js
localStorage.removeItem('companion_v2');
location.reload();
```

---

## 技术实现

### 技术栈

- 纯 HTML + CSS + 原生 JavaScript
- 单文件，零依赖
- 无后端，无数据收集
- 数据存 localStorage

### 核心模块

```
1. 数据层（store）
   - posts：帖子列表
   - chats：按人设分桶的聊天记录
   - unread / unreadDay：未读管理
   - weights / ignored：权重系统

2. 人设系统
   - characters：3 位人设的完整数据
   - detectMood()：情绪识别
   - weightedPick()：加权挑选

3. 交互系统
   - publishPost()：发布 + 触发回复
   - sendMessage()：发送消息 + 触发回复
   - openChat()：进入聊天
   - showProfile()：查看主页

4. 持久化
   - load() / save()
   - checkUnreadExpiry()：次日清空
```

### 关键函数

**publishPost()**：

```js
function publishPost() {
  const content = input.value.trim();
  if (!content) return;

  const post = {
    id: uid(),
    content,
    mood: detectMood(content),
    time: now(),
    replies: []
  };
  store.posts.unshift(post);
  save();
  renderPosts();

  // 按权重挑 3 人
  const picked = weightedPick(Object.keys(characters), 3);
  picked.forEach((key, i) => {
    const delay = 1000 + i * 2500 + Math.random() * 1500;
    setTimeout(() => {
      const c = characters[key];
      const text = c.replies[post.mood] || c.replies.default;
      post.replies.push({ charKey: key, text, time: now(), read: false });
      markUnread(key);
      save();
      renderPosts();
      updateBadge();
    }, delay);
  });
}
```

**detectMood()**：

```js
const MOOD_KEYWORDS = {
  tired: ['累', '疲惫', '困', '加班', '熬夜', '撑不住'],
  sad: ['难过', '伤心', '哭', '委屈', '失落', '崩溃'],
  happy: ['开心', '高兴', '快乐', '兴奋', '太好了', '棒'],
  anxious: ['焦虑', '紧张', '压力', '担心', '害怕', '慌']
};

function detectMood(text) {
  if (!text) return 'default';
  for (const mood in MOOD_KEYWORDS) {
    if (MOOD_KEYWORDS[mood].some(k => text.includes(k))) return mood;
  }
  return 'default';
}
```

---

## 如何自定义

### 修改人设

在 `characters` 对象里修改：

```js
const characters = {
  luna: {
    name: 'Luna',
    avatar: 'L',
    color: '#FF9A9E',
    age: 27,
    // ...
  }
};
```

### 新增一位人设

1. 在 `characters` 里加一个新 key
2. 复制已有结构，填内容
3. 无需修改 HTML，所有列表、主页、聊天会自动渲染
4. 权重初始化为 1.0

### 修改情绪关键词

在 `MOOD_KEYWORDS` 里增删：

```js
const MOOD_KEYWORDS = {
  tired: ['累', '疲惫', ...],
  // 新增其他情绪
};
```

### 修改回复延迟

在 `publishPost()` 里修改：

```js
const delay = 1000 + i * 2500 + Math.random() * 1500;
```

### 修改权重变化

在 `penalize()` 和 `reward()` 里修改：

```js
function penalize(charKey) {
  store.weights[charKey] = Math.max(0.1, (store.weights[charKey] || 1.0) * 0.6);
}

function reward(charKey) {
  store.weights[charKey] = Math.min(2.0, (store.weights[charKey] || 1.0) * 1.2);
}
```

### 修改主题色

主色是 `#333`，全局替换即可：

```bash
sed -i '' 's/#333/#10b981/g' index.html
```

---

## 部署到 GitHub Pages

### 1. 文件准备

```
companion/
├── index.html       # 把代码保存为 index.html
└── README.md
```

### 2. 上传到 GitHub

1. 新建仓库 `companion`（Public）
2. `Add file` → `Upload files` → 拖入所有文件
3. `Commit changes`

### 3. 开启 Pages

1. 仓库 → `Settings` → `Pages`
2. Source 选 `Deploy from a branch`
3. Branch 选 `main`，目录选 `/ (root)`
4. 点 `Save`，等 1~2 分钟
5. 访问 `https://你的用户名.github.io/companion/`

### 4. 后续更新

```bash
git add .
git commit -m "update"
git push
```

Pages 会自动重新部署。

---

## 常见问题

### Q1：人设会真的记住我吗

会。所有聊天记录存在 localStorage 里，刷新不丢。

但不会跨设备同步（因为存在本地）。

### Q2：为什么有时候会收到重复的回复

因为同一情绪下，人设的回复是固定的。

比如每次发"今天好累"，Luna 都会回"辛苦啦\n累的话就先歇一会儿……"。

如果需要更自然的回复，见「后续规划」中的接入 LLM。

### Q3：权重系统怎么工作的

- 初始权重都是 1.0
- 忽略某人 → 权重 × 0.6
- 回应某人 → 权重 × 1.2
- 权重越高，被挑中概率越大

### Q4：未读为什么突然消失了

因为未读是**次日自动清空**的。

如果你昨天没打开，今天打开时清空。这是有意设计，避免未读焦虑。

### Q5：数据会丢失吗

以下情况会丢失：

- 清除浏览器数据
- 使用无痕模式
- 换浏览器或设备

### Q6：能改成其他人设吗

可以。在 `characters` 对象里修改即可，不需要改 HTML。

### Q7：为什么聊天输入框只在聊天页显示

因为 `.chat-input-area` 只在 `page === 'chat'` 时激活。

这样避免在首页、发现页时输入框遮挡内容。

### Q8：能接入大模型 API 吗

可以。把 `sendMessage()` 里的本地回复替换为：

```js
async function getAIReply(charKey, userText) {
  const res = await fetch('https://your-api.com/chat', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ character: charKey, message: userText })
  });
  const data = await res.json();
  return data.reply;
}
```

详见「后续规划」。

---

## 已知限制

- **回复是硬编码**：同一情绪下回复固定，重复感明显
- **只支持文字**：没有图片、语音、视频
- **无用户系统**：无法跨设备同步
- **无 API 接入**：不能接入真实 LLM
- **权重系统是简化版**：未考虑时间衰减
- **localStorage 容量有限**：约 5MB，数据量大后需换 IndexedDB
- **无错误边界**：如果某个函数报错，页面可能卡住
- **人设数量固定**：3 位

---

## 后续规划

| 优先级 | 功能 | 说明 |
|--------|------|------|
| P0 | 接入 LLM API | 替换硬编码回复 |
| P0 | 打字中动画 | 增强"真人感" |
| P1 | 语音输入 / 播报 | 更自然的交互 |
| P1 | 人设自定义 | 用户创建新角色 |
| P1 | 对话导出 | 复制 / 下载 txt |
| P2 | 情绪趋势图 | 7 天心情曲线 |
| P2 | 群聊模式 | 多人设同时在线 |
| P2 | 用户系统 + 云同步 | 跨设备 |
| P3 | 订阅 / 付费 | 商业模式 |
| P3 | 数据埋点 | 分析用户行为 |

### 接入大模型 API（P0）

把 `sendMessage()` 里的本地回复替换为 LLM 调用：

```js
async function getAIReply(charKey, userText, context) {
  const character = characters[charKey];
  const prompt = `
    你是 ${character.name}，${character.age} 岁，${character.city}人，职业是${character.job}。
    你的性格：${character.tags.join('、')}。
    你的说话风格：${character.bio}
    
    用户说："${userText}"
    
    请用你的风格回复，1-3 句话。
    不要暴露你是 AI。
    不要使用套话。
  `;
  
  const res = await fetch('https://your-api.com/chat', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ prompt })
  });
  const data = await res.json();
  return data.reply;
}
```

---

## 许可证

MIT