# 小红书图文卡片 — 风格预设参考

基于 frontend-slides 的 12 种预设，针对小红书 **3:4 竖版卡片**优化。
竖版卡片特点：内容区更高、字体需更大、留白更重要。

> ⚠️ **禁止侧边标签耳**：不得在卡片侧边生成竖向彩色标签条，永久禁用。
> ⚠️ **标题居中**：封面/引用卡标题强制水平居中，不偏侧。

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
    --bg-primary: #1a1a1a;
    --bg-gradient: linear-gradient(135deg, #1a1a1a 0%, #2d2d2d 50%, #1a1a1a 100%);
    --card-bg: #FF5722;        /* 橙红撞色 */
    --text-primary: #ffffff;
    --text-on-card: #1a1a1a;
}
```

**特征元素：**
- 大色块作为视觉焦点（橙/珊瑚/鲜艳强调色）
- 大号章节编号（01、02…）装饰
- 网格布局精确对齐
- 内容卡：左侧竖向彩色条 + 数字序号高亮

---

### 2. Electric Studio（电气工作室）

**气质：** 大胆干净、专业高对比、适合商业/职场/品牌内容

**字体：**
- 全局：`Manrope`（800/400/500）

**色彩：**
```css
:root {
    --bg-dark: #0a0a0a;
    --bg-white: #ffffff;
    --accent-blue: #4361ee;
    --text-dark: #0a0a0a;
    --text-light: #ffffff;
}
```

**特征元素：**
- 竖向双色分割面板（深色/白色）
- 面板边缘强调色竖条
- 引用大字作为视觉主角
- 极简自信的留白节奏

---

### 3. Creative Voltage（创意电压）

**气质：** 大胆有冲劲、复古现代感、适合创意/年轻/活力内容

**字体：**
- 标题：`Syne`（700/800）
- 辅助：`Space Mono`（400/700）

**色彩：**
```css
:root {
    --bg-primary: #0066ff;     /* 电蓝 */
    --bg-dark: #1a1a2e;
    --accent-neon: #d4ff00;    /* 霓虹黄 */
    --text-light: #ffffff;
}
```

**特征元素：**
- 电蓝 + 霓虹黄高对比配色
- 半调网点纹理图案
- 霓虹徽章/标注框
- 混合大小写排版营造能量感

---

### 4. Dark Botanical（深色植物）

**气质：** 优雅沉稳、有感染力、适合情绪/日记/生活方式

**字体：**
- 标题：`Cormorant`（400/600，衬线）
- 正文：`IBM Plex Sans`（300/400）

**色彩：**
```css
:root {
    --bg-primary: #0f0f0f;
    --text-primary: #e8e4df;
    --text-secondary: #9a9590;
    --accent-warm: #d4a574;
    --accent-pink: #e8b4b8;
    --accent-gold: #c9b896;
}
```

**特征元素：**
- 模糊光晕背景（radial-gradient blur blob，重叠排列）
- 暖色调强调（粉/金/赤陶）
- 细竖向强调线
- 斜体签名式排版
- 封面竖版居中，意境感排版

---

### 5. Neon Cyber（霓虹赛博）

**气质：** 未来感、科技、适合测评/数码/工具类

**字体：**
- 标题：`Clash Display`（Fontshare）
- 正文：`Satoshi`（Fontshare）

**色彩：**
```css
:root {
    --bg: #0a0f1c;             /* 深海军蓝 */
    --accent-cyan: #00ffcc;
    --accent-magenta: #ff00aa;
    --text-primary: #e8e8ff;
}
```

**特征元素：**
- 粒子/星点背景效果
- 霓虹发光效果（text-shadow）
- 网格线装饰
- 数字/数据用霓虹色高亮
- 卡片边框用渐变描边

---

### 6. Terminal Green（终端绿）

**气质：** 开发者美学、极客风、适合技术科普/代码/效率工具

**字体：**
- 全局：`JetBrains Mono`（等宽，纯代码感）

**色彩：**
```css
:root {
    --bg: #0d1117;             /* GitHub深色 */
    --accent-green: #39d353;   /* 终端绿 */
    --text-primary: #c9d1d9;
    --text-dim: #484f58;
}
```

**特征元素：**
- 扫描线纹理效果
- 闪烁光标动效
- 代码语法高亮风格排版
- `>` 或 `$` 命令行提示符装饰

---

## 浅色系

### 7. Notebook Tabs（笔记本）

**气质：** 编辑感、整洁、适合知识科普/学习笔记

**字体：**
- 标题：`Bodoni Moda`（经典编辑）
- 正文：`DM Sans`（400/500）

**色彩：**
```css
:root {
    --bg-outer: #2d2d2d;
    --bg-page: #f8f6f1;
    --text-primary: #1a1a1a;
    --accent-mint: #98d4bb;
    --accent-lavender: #c7b8ea;
    --accent-pink: #f4b8c5;
    --accent-sky: #a8d8ea;
    --accent-cream: #ffe6a7;
}
```

**特征元素：**
- 奶油纸张质感卡片（带微阴影）
- 彩色分区色块作为顶部/底部横向色带（**禁止做侧边标签耳**）
- 左侧装饰性活页圆孔
- 手写感细节或笔记风格排版

---

### 8. Pastel Geometry（柔色几何）

**气质：** 友好现代、清新可爱、适合生活/学习/软性干货

**字体：**
- 全局：`Plus Jakarta Sans`（700/800/400/500）

**色彩：**
```css
:root {
    --bg-primary: #c8d9e6;     /* 粉蓝背景 */
    --card-bg: #faf9f7;
    --pill-pink: #f0b4d4;
    --pill-mint: #a8d4c4;
    --pill-sage: #5a7c6a;
    --pill-lavender: #9b8dc4;
    --pill-violet: #7c6aad;
}
```

**特征元素：**
- 圆角卡片，柔和投影
- 几何装饰形状（圆/半圆/矩形）用强调色填充
- 圆润 badge 标签点缀
- 统一圆角 CTA 按钮

---

### 9. Vintage Editorial（复古杂志）

**气质：** 有个性、文艺、适合生活/美食/旅行/观点

**字体：**
- 标题：`Fraunces`（700/900，特色衬线）
- 正文：`Work Sans`（400/500）

**色彩：**
```css
:root {
    --bg-cream: #f5f3ee;
    --text-primary: #1a1a1a;
    --text-secondary: #555;
    --accent-warm: #e8d4c0;
    --accent-red: #d4522a;     /* 复古橙红 */
}
```

**特征元素：**
- 几何线条分割区域（圆形轮廓 + 线 + 点，**纯CSS绘制，无插图**）
- 粗边框 CTA 框
- 大号数字作为装饰元素
- 口语化、有个性的文案风格

---

### 10. Split Pastel（马卡龙双拼）

**气质：** 可爱清新、活泼、适合美妆/穿搭/生活分享

**字体：**
- 全局：`Outfit`（700/800，圆润现代）

**色彩：**
```css
:root {
    --bg-peach: #f5e6dc;       /* 蜜桃 */
    --bg-lavender: #e4dff0;    /* 薰衣草 */
    --text-dark: #1a1a1a;
    --badge-mint: #c8f0d8;
    --badge-yellow: #f0f0c8;
    --badge-pink: #f0d4e0;
}
```

**特征元素：**
- 竖向双色分割背景（上桃色/下紫色）
- 可爱 badge 徽章（含小图标）
- 右侧面板网格纹理覆盖
- 圆角 CTA 按钮

---

### 11. Swiss Modern（瑞士现代）

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

**特征元素：**
- 可见网格线布局，精确对齐
- 非对称布局，几何形状装饰
- 红色仅用于关键字/数字高亮

---

### 12. Paper & Ink（纸墨）

**气质：** 编辑感、文学性、沉思、适合深度文章/读书笔记/观点

**字体：**
- 标题：`Cormorant Garamond`（衬线）
- 正文：`Source Serif 4`

**色彩：**
```css
:root {
    --bg-cream: #faf9f7;       /* 温暖奶油 */
    --text-primary: #1a1a1a;   /* 炭黑 */
    --accent-crimson: #c41e3a; /* 深红 */
}
```

**特征元素：**
- 首字下沉（drop cap）装饰
- 拉出引用块（pull quote）
- 精致水平分隔线
- 书籍排版般的行间距和字距

---

## 快速选择指南（按内容类型）

| 内容类型 | 推荐预设 |
|---------|---------|
| 干货/步骤/方法论 | Bold Signal、Swiss Modern、Notebook Tabs |
| 情绪/日记/生活 | Dark Botanical、Vintage Editorial、Paper & Ink |
| 美妆/穿搭/好物 | Split Pastel、Pastel Geometry |
| 数码/科技/测评 | Neon Cyber、Terminal Green、Electric Studio |
| 职场/商业/品牌 | Electric Studio、Swiss Modern、Bold Signal |
| 创意/年轻/活力 | Creative Voltage、Split Pastel |
| 美食/旅行/探店 | Vintage Editorial、Split Pastel |
| 读书/深度/观点 | Paper & Ink、Dark Botanical、Vintage Editorial |
| 知识科普/学习 | Notebook Tabs、Swiss Modern、Dark Botanical |

---

## 字体速查表

| 预设 | 标题字体 | 正文字体 | 来源 |
|------|---------|---------|------|
| Bold Signal | Archivo Black | Space Grotesk | Google |
| Electric Studio | Manrope | Manrope | Google |
| Creative Voltage | Syne | Space Mono | Google |
| Dark Botanical | Cormorant | IBM Plex Sans | Google |
| Neon Cyber | Clash Display | Satoshi | Fontshare |
| Terminal Green | JetBrains Mono | JetBrains Mono | JetBrains |
| Notebook Tabs | Bodoni Moda | DM Sans | Google |
| Pastel Geometry | Plus Jakarta Sans | Plus Jakarta Sans | Google |
| Vintage Editorial | Fraunces | Work Sans | Google |
| Split Pastel | Outfit | Outfit | Google |
| Swiss Modern | Archivo | Nunito | Google |
| Paper & Ink | Cormorant Garamond | Source Serif 4 | Google |

---

## CSS 注意事项

**负值 CSS 函数** — 必须用 `calc(-1 * clamp(...))` 而不是 `-clamp(...)`

**字体大小** — 小红书卡片比幻灯片需要更大的字体基准，所有尺寸用 `cqw` 不用 `vw`

**留白** — 小红书用户快速浏览，充足留白比信息密度更重要

**禁止使用：** Inter、Roboto、Arial（系统字体）；紫色渐变白底（泛滥）；文字小于 `clamp(0.6rem, 2.6cqw, 1.4rem)`
