# 小红书图文卡片 — 动效参考

小红书卡片以**截图导出**为最终形态，动效的主要作用是：
1. 在浏览器预览时产生视觉愉悦感
2. 帮助用户确认内容层级和节奏

**原则：** 动效服务于内容，不喧宾夺主。首选 entrance 动效（进场），少用 loop（循环）动效。

---

## 情绪 → 动效对照表

| 情绪 | 动效风格 | 视觉提示 |
|------|---------|---------|
| 专业/可信 | 快速轻微淡入（0.3-0.5s）| 深色/白色，精确对齐 |
| 活力/兴奋 | 弹性上移（spring easing）| 亮色撞色，大字 |
| 冷静/清晰 | 极慢淡入（0.8-1s）| 大留白，浅色 |
| 感染/情绪 | 慢速模糊消散（blur fade）| 暖色光晕背景 |

---

## 入场动效代码

```css
/* === Fade Up（最通用）=== */
.reveal {
    opacity: 0;
    transform: translateY(20px);
    transition: opacity 0.6s cubic-bezier(0.16, 1, 0.3, 1),
                transform 0.6s cubic-bezier(0.16, 1, 0.3, 1);
}
.card.visible .reveal { opacity: 1; transform: translateY(0); }

/* === Scale In（封面标题感）=== */
.reveal-scale {
    opacity: 0;
    transform: scale(0.92);
    transition: opacity 0.7s cubic-bezier(0.16, 1, 0.3, 1),
                transform 0.7s cubic-bezier(0.16, 1, 0.3, 1);
}
.card.visible .reveal-scale { opacity: 1; transform: scale(1); }

/* === Blur Fade（意境感背景/光晕）=== */
.reveal-blur {
    opacity: 0;
    filter: blur(8px);
    transition: opacity 0.9s ease, filter 0.9s ease;
}
.card.visible .reveal-blur { opacity: 1; filter: blur(0); }

/* === Slide from Left（列表项逐条进场）=== */
.reveal-left {
    opacity: 0;
    transform: translateX(-24px);
    transition: opacity 0.5s cubic-bezier(0.16, 1, 0.3, 1),
                transform 0.5s cubic-bezier(0.16, 1, 0.3, 1);
}
.card.visible .reveal-left { opacity: 1; transform: translateX(0); }

/* === 交错延迟（stagger）=== */
.d1 { transition-delay: 0.05s; }
.d2 { transition-delay: 0.15s; }
.d3 { transition-delay: 0.28s; }
.d4 { transition-delay: 0.42s; }
.d5 { transition-delay: 0.56s; }
.d6 { transition-delay: 0.70s; }
```

---

## 背景效果

```css
/* === 光晕背景（Dark Botanical 风格）=== */
.blob {
    position: absolute;
    border-radius: 50%;
    filter: blur(80px);
    pointer-events: none;
    z-index: 0;
}
/* 使用：<div class="blob" style="width:60vw;height:60vw;top:-10vw;right:-10vw;
   background:radial-gradient(circle, rgba(212,165,116,0.2) 0%, transparent 70%)"></div> */

/* === 网格背景（Swiss Modern 风格）=== */
.grid-bg {
    background-image:
        linear-gradient(rgba(0,0,0,0.04) 1px, transparent 1px),
        linear-gradient(90deg, rgba(0,0,0,0.04) 1px, transparent 1px);
    background-size: clamp(20px, 5vw, 40px) clamp(20px, 5vw, 40px);
}

/* === 纸张纹理（Notebook 风格，用 SVG noise）=== */
/* 用 background-image: url("data:image/svg+xml,...") 实现，保持文件自足 */
```

---

## Intersection Observer（触发入场动效）

```javascript
// 每张卡片进入视口时添加 .visible 类
const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            entry.target.classList.add('visible');
        }
    });
}, { threshold: 0.3 });

document.querySelectorAll('.card').forEach(card => observer.observe(card));
```

---

## 不推荐在小红书卡片中使用的动效

- **Loop 动效**（旋转/弹跳循环）— 截图导出后无意义，预览时分散注意力
- **Particle System**（粒子系统）— 性能消耗大，导出无效
- **3D Perspective Tilt** — 在竖版卡片上效果差
- **复杂路径动画** — 卡片内容区小，路径动画无法展示完整

---

## 故障排查

| 问题 | 解决方案 |
|------|---------|
| 动效不触发 | 检查 IntersectionObserver threshold；确认 `.visible` 已添加 |
| 字体未加载 | 检查 Google Fonts URL；确认字体名与 CSS 一致 |
| 卡片溢出 | 检查 `.card` 是否有 `overflow: hidden`；内容是否超限 |
| 宽屏下比例变形 | 检查 card-base.css 的 `@media (min-width: 600px)` 是否正确 |
| 截图时动效未完成 | 截图前等待动效结束（约1s），或截图工具设置延迟 |
