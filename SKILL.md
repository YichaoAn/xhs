---
name: xhs-image-cards
description: 生成高质量小红书图文内容，每张图片严格为 3:4 竖版比例（如 1080×1440px），关键信息全部在图片中呈现，支持多图卡片组（封面+内容页+结尾页）。当用户想为小红书创作图文笔记、生成配图、制作小红书图文素材、或提到"小红书图片/图文/图卡"时使用。继承 frontend-slides 的设计能力：零依赖单 HTML 文件、Show Don't Tell 风格预览、12 种视觉预设、内联编辑、动画效果。
---

# 小红书图文 Skill（xhs-image-cards）

生成用于小红书发布的高质量图文卡片组，每张图严格 **3:4 竖版比例**，关键内容全部在图片中呈现。

## 核心约束（NON-NEGOTIABLE）

1. **3:4 比例锁死** — 每张卡片 `width: 100vw; height: calc(100vw * 4/3)`，桌面端 `min(420px, 90vw)` 居中。永远不用 `100vh`。
2. **container-type 必加** — `.card` 必须有 `container-type: inline-size`，开启容器查询。
3. **cqw 不是 vw** — 所有字体和间距用 `cqw`（容器查询宽度），**绝不用 vw**。原因：桌面卡片宽 420px，但视口可能 1440px，用 vw 字体会溢出。
4. **内容自足** — 每张图片独立传达信息，无需配合正文。
5. **零依赖** — 单 HTML 文件，所有 CSS/JS inline（字体链接除外）。
6. **不溢出** — 内容超出卡片？拆成多张，绝不压缩或滚动。
7. **独特设计** — 禁止通用"AI slop"美学；每套卡片必须有鲜明视觉个性。
8. **禁止标签耳（Notebook Tabs 侧边彩色标签）** — 不得在卡片侧边生成竖向彩色标签条/标签耳结构。该设计会占用内容宽度且视觉干扰强，永久禁用。
9. **标题必须居中** — 封面卡和引用卡的标题/大字，必须水平居中（`text-align: center; align-items: center`）。内容列表卡的正文列表项可以左对齐，但标题行本身仍须居中。

## 小红书卡片结构规范

| 卡片类型 | 内容上限 | 用途 |
|---------|---------|------|
| 封面卡（Cover） | 标题（大字）+ 副标题 + 账号名/话题 | 第1张，决定点击率 |
| 内容卡（Content） | 1个核心观点 + 2-4条支撑信息 | 第2-N张，传达关键信息 |
| 列表卡（List） | 标题 + 最多5条列表项 | 干货/步骤/清单类内容 |
| 引用卡（Quote） | 1句核心金句（最多3行）+ 来源 | 情绪/观点类内容 |
| 结尾卡（Closing） | 互动引导语 + 话题标签 + 账号信息 | 最后1张，引导评论/关注 |

**内容卡密度：** 宁可多张，不要一张塞满。每张只讲一件事。

---

## Phase 0: 检测模式

- **Mode A: 新建图文组** — 从零创作。→ Phase 1
- **Mode B: 基于现有内容生成** — 用户已提供文案/素材/URL内容。→ Phase 1（跳过 Q1 素材收集）
- **Mode C: 修改已有卡片** — 读取现有 HTML，按规则修改。→ 见下方 Mode C 规则

### Mode C：修改规则

修改时卡片溢出是最大风险：

1. **添加内容前：** 清点已有元素数量，对照密度限制
2. **任何修改后验证：** `.card` 有 `overflow: hidden`；新元素用 `cqw`；内容在 420px 宽容器内完整显示
3. **若修改会导致溢出：** 主动拆分成多张卡片，并告知用户
4. **绝不等用户发现溢出才处理** — 主动预防

---

## Phase 1: 内容 & 偏好收集

**一次性** 询问以下所有问题（让用户一并填写，不要分步骤问）：

---

**Q1 — 内容素材**（若用户已提供则跳过）
图文的核心内容是什么？可以直接贴文案、笔记、或描述主题。

**Q2 — 卡片数量**
建议：3-5 张（短）/ 6-9 张（中）/ 10张以上（长图文）

**Q3 — 内容类型**（帮助组织结构）
- 干货实操型（步骤/清单/方法论）
- 情绪共鸣型（故事/感悟/吐槽）
- 观点站队型（支持/反对/争议）
- 知识科普型（数据/解释/对比）

**Q4 — 内联编辑**（重要，必须询问）
生成后是否需要在浏览器里直接修改文字？
- **是（推荐）** — 可在浏览器点击文字直接编辑，`E` 键切换，`⌘S` 导出修改后 HTML，自动 localStorage 保存
- **否** — 只用于浏览/截图，文件更小

> ⚠️ 若用户未明确回答，**默认选「是（推荐）」**。
> 若用户明确选「否」，**不生成任何** edit-hotzone / edit-toggle / InlineEditor 相关代码。
> **记住此选择**，Phase 3 生成时严格执行。

---

收到回答后，整理一份**内容大纲**（卡片列表 + 每张核心信息），展示给用户确认：

> 📋 **内容大纲确认**
> 1. 封面：[标题]
> 2. [卡片类型]：[核心信息]
> …
> N. 结尾：[互动引导]
>
> 这个结构OK吗？还是需要调整？

用户确认后 → **进入 Phase 2**

---

## Phase 2: 风格选择（Show, Don't Tell）

> 这是"给你看，不是告诉你"的阶段。大多数人无法用语言描述设计偏好，但一眼就能判断对不对。

### Step 2.1: 情绪选择

询问（最多选2个）：
> 你希望看图文的人有什么感觉？
> - 专业可信 — 认真靠谱，值得收藏
> - 活力兴奋 — 有冲劲，想点进来
> - 冷静清晰 — 思路清楚，一眼看懂
> - 感染有力 — 有情绪，有共鸣
> - 创意张力 — 有设计感，眼前一亮
> - 文艺温柔 — 细腻克制，有品味

### Step 2.2: 生成3个单卡预览

基于情绪选择，从以下对应关系挑选3种差异明显的预设：

| 情绪 | 推荐预设 |
|------|---------|
| 专业可信 | Bold Signal、Electric Studio、Swiss Modern |
| 活力兴奋 | Creative Voltage、Neon Cyber、Split Pastel |
| 冷静清晰 | Swiss Modern、Notebook Tabs、Paper & Ink |
| 感染有力 | Dark Botanical、Vintage Editorial、Pastel Geometry |
| 创意张力 | Creative Voltage、Electric Studio、Bold Signal |
| 文艺温柔 | Paper & Ink、Dark Botanical、Vintage Editorial |

读取 [references/style-presets.md](references/style-presets.md) 获取每个预设的字体/颜色规范。

**预览生成要求：**
- 保存至 `.claude-design/card-previews/style-a.html`、`style-b.html`、`style-c.html`
- 每个预览必须是 **3:4 比例卡片**（使用 `card-base.css` 规范），展示真实字体/颜色/排版/入场动效
- 预览内容用实际的用户内容（封面卡），不用假占位符
- 每个预览 ~50-100 行，自包含
- 生成完后**逐个 `open` 打开**（三个都要打开）：
  ```bash
  open .claude-design/card-previews/style-a.html
  open .claude-design/card-previews/style-b.html
  open .claude-design/card-previews/style-c.html
  ```

### Step 2.3: 用户选择

询问：
> 三个风格你更喜欢哪个？
> - **Style A**：[预设名称] — [一句描述]
> - **Style B**：[预设名称] — [一句描述]
> - **Style C**：[预设名称] — [一句描述]
> - **混合元素** — 说说想取哪个的什么部分

风格确认后 → **进入 Phase 3**

---

## Phase 3: 生成卡片组

**生成前，读取以下支持文件：**
- [references/card-base.css](references/card-base.css) — 必须 include 的 3:4 基础样式（含 container-type、cqw 规范）
- [references/animation-patterns.md](references/animation-patterns.md) — 动效参考
- [references/html-template.md](references/html-template.md) — HTML 结构、JS 功能、内联编辑规范

**关键技术要求：**

```
.card {
  container-type: inline-size;   /* REQUIRED */
  width: 100vw;
  height: calc(100vw * 4 / 3);
  overflow: hidden;
  scroll-snap-align: start;
}

@media (min-width: 600px) {
  .card {
    width: min(420px, 90vw);
    height: calc(min(420px, 90vw) * 4 / 3);
  }
}
```

**⚠️ 字体缩放（NON-NEGOTIABLE）：**
- ✅ 正确：`font-size: clamp(0.8rem, 3.5cqw, 1.1rem)`
- ❌ 错误：`font-size: clamp(0.8rem, 3.5vw, 1.1rem)`
- 参考：390px 卡片时 `3.5cqw ≈ 14px`；1440px 视口下 `3.5vw ≈ 50px`（溢出）

**内联编辑（按 Q4 决定）：**
- 用户选「是」→ 生成完整 InlineEditor（edit-hotzone + edit-toggle + localStorage + ⌘S 导出）
- 用户选「否」→ 完全不生成上述代码
- 实现规范：参照 [references/html-template.md](references/html-template.md)，使用 JS hover + 400ms delay（**禁止用 CSS `~` sibling selector**）

**JS 功能必须包含：**
- `CardPresentation` 类：键盘导航（方向键/空格）、触控滑动、滚轮、进度条、导航点、卡片计数器
- `IntersectionObserver`：卡片进入视口时添加 `.visible` 触发入场动效

---

## Phase 4: 交付

1. **清理临时文件** — 删除 `.claude-design/card-previews/` 目录
2. **打开文件** — `open [filename].html`
3. **汇报交付信息**：
   - 文件路径、风格名称、卡片总数
   - 导航方式：方向键 / 空格 / 滚轮 / 右侧导航点
   - 编辑方式（若启用）：鼠标移到左上角 → 出现 ✏ 编辑按钮，或按 `E` 键；`⌘S` 导出修改后的 HTML
   - 截图导出：Chrome DevTools → `Cmd+Shift+P` → "Capture node screenshot" 选中 `.card`，或用 GoFullPage 插件

---

## 支持文件

| 文件 | 用途 | 何时读取 |
|------|------|--------|
| [references/style-presets.md](references/style-presets.md) | 12种视觉预设（字体/颜色/特征元素） | Phase 2 生成预览时 |
| [references/card-base.css](references/card-base.css) | 3:4 比例基础 CSS（含 cqw、container-type 规范） | Phase 3 生成时 |
| [references/animation-patterns.md](references/animation-patterns.md) | 动效模式参考 | Phase 3 生成时 |
| [references/html-template.md](references/html-template.md) | HTML 结构、JS 功能、内联编辑规范 | Phase 3 生成时 |
