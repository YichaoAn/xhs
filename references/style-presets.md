# 小红书图文卡片 — 风格预设参考

基于 frontend-slides 的12种预设，针对小红书 **3:4 竖版卡片**优化。
竖版卡片特点：内容区更高、字体需更大、留白更重要。

---

## 深色系

### 1. Bold Signal（撞色信号）

**气质：** 有冲击力、专业有力、适合干货/结论类内容

**字体：**
- 标题：`Archivo Black`（900）
- 正文：`Space Grotesk`（400/500）

**色彩：**
```css
:root {
    --bg: #1a1a1a;
    --accent: #FF5722;         /* 橙红撞色 */
    --text-primary: #ffffff;
    --text-on-accent: #1a1a1a;
}
```

**小红书适配：**
- 封面：大色块占上1/3，白色大字
- 内容卡：左侧竖向彩色条，数字序号高亮

---

### 2. Dark Botanical（深色植物）

**气质：** 优雅沉稳、有感染力、适合情绪/日记/生活方式

**字体：**
- 标题：`Cormorant`（600，衬线）
- 正文：`IBM Plex Sans`（300/400）

**色彩：**
```css
:root {
    --bg: #0f0f0f;
    --accent-gold: #d4a574;
    --accent-pink: #e8b4b8;
    --text-primary: #e8e4df;
    --text-secondary: #9a9590;
}
```

**小红书适配：**
- 背景用模糊光晕（radial-gradient blur blob）
- 封面：竖版居中，意境感排版

---

### 3. Neon Cyber（霓虹赛博）

**气质：** 未来感、科技、适合测评/数码/工具类

**字体：**
- 全局：`JetBrains Mono`（代码感）或 `Syne`

**色彩：**
```css
:root {
    --bg: #0a0f1c;
    --accent-cyan: #00ffcc;
    --accent-magenta: #ff00aa;
    --text-primary: #e8e8ff;
}
```

**小红书适配：**
- 数字/数据用霓虹色高亮
- 卡片边框可用渐变描边

---

## 浅色系

### 4. Notebook Tabs（笔记本标签）

**气质：** 编辑感、整洁、适合知识科普/学习笔记

**字体：**
- 标题：`Bodoni Moda`（经典编辑）
- 正文：`DM Sans`（400/500）

**色彩：**
```css
:root {
    --bg-outer: #f0ede8;
    --bg-page: #faf9f6;
    --text-primary: #1a1a1a;
    --tab-mint: #98d4bb;
    --tab-lavender: #c7b8ea;
    --tab-pink: #f4b8c5;
}
```

**小红书适配：**
- 纸张质感背景
- 彩色分区色块（作为顶部或底部横向色带，不做侧边标签耳）
- 笔记风格插图或手写感细节

---

### 5. Vintage Editorial（复古杂志）

**气质：** 有个性、文艺、适合生活/美食/旅行

**字体：**
- 标题：`Fraunces`（700/900，特色衬线）
- 正文：`Work Sans`（400/500）

**色彩：**
```css
:root {
    --bg: #f5f3ee;
    --text-primary: #1a1a1a;
    --text-secondary: #666;
    --accent-warm: #d4522a;    /* 复古橙红 */
    --accent-line: #1a1a1a;
}
```

**小红书适配：**
- 几何线条分割区域（不用插图）
- 大号数字作为装饰元素

---

### 6. Split Pastel（马卡龙双拼）

**气质：** 可爱清新、活泼、适合美妆/穿搭/生活分享

**字体：**
- 全局：`Outfit`（700/800，圆润现代）

**色彩：**
```css
:root {
    --bg-left: #f5e6dc;        /* 蜜桃 */
    --bg-right: #e4dff0;       /* 薰衣草 */
    --text-dark: #1a1a1a;
    --badge-mint: #c8f0d8;
    --badge-yellow: #f0f0c8;
}
```

**小红书适配：**
- 封面用竖向双色分割（上桃色下紫色）
- 小标签 badge 点缀

---

### 7. Swiss Modern（瑞士现代）

**气质：** 极简精准、高级感、适合职场/商业/效率

**字体：**
- 标题：`Archivo`（800）
- 正文：`Nunito`（400）

**色彩：**
```css
:root {
    --bg: #ffffff;
    --text-primary: #0a0a0a;
    --accent-red: #ff3300;
    --grid-line: rgba(0,0,0,0.06);
}
```

**小红书适配：**
- 网格布局，精确对齐
- 红色仅用于关键字/数字高亮

---

## 快速选择指南（按内容类型）

| 内容类型 | 推荐预设 |
|---------|---------|
| 干货/步骤/方法论 | Bold Signal、Swiss Modern、Notebook Tabs |
| 情绪/日记/生活 | Dark Botanical、Vintage Editorial |
| 美妆/穿搭/好物 | Split Pastel、Notebook Tabs |
| 数码/科技/测评 | Neon Cyber、Swiss Modern |
| 职场/考公/学习 | Swiss Modern、Bold Signal、Dark Botanical |
| 美食/旅行/探店 | Vintage Editorial、Split Pastel |

---

## CSS 注意事项

**负值 CSS 函数** — 必须用 `calc(-1 * clamp(...))` 而不是 `-clamp(...)`

**字体大小** — 小红书卡片比幻灯片需要更大的字体基准，所有尺寸已在 card-base.css 中调大约20%

**留白** — 小红书用户快速浏览，充足留白比信息密度更重要

**禁止使用：** Inter、Roboto、Arial（系统字体）；紫色渐变白底（泛滥）；文字小于 `clamp(0.6rem, 2.6vw, 1.4rem)`
