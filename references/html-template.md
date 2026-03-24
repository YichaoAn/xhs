# 小红书图文卡片 — HTML 结构规范

## 文件结构

单一 HTML 文件，所有 CSS/JS inline，无外部依赖（字体除外）。

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>[主题] — 小红书图文</title>
    <!-- Google Fonts / Fontshare — 唯一允许的外部依赖 -->
    <link href="https://fonts.googleapis.com/css2?family=...&display=swap" rel="stylesheet">
    <style>
        /* === THEME VARIABLES === */
        :root { /* 颜色/字体变量 */ }

        /* --- 粘贴 card-base.css 完整内容 --- */

        /* === CARD-SPECIFIC STYLES === */
        /* 各类型卡片的独特样式 */
    </style>
</head>
<body>
    <!-- Progress bar -->
    <div class="progress-bar" id="progressBar"></div>

    <!-- Nav dots（右侧） -->
    <nav class="nav-dots" id="navDots"></nav>

    <!-- Card counter（底部中央） -->
    <div class="card-counter" id="cardCounter"></div>

    <!-- Inline edit controls（可选） -->
    <div class="edit-hotzone" id="editHotzone"></div>
    <button class="edit-toggle" id="editToggle">✏ 编辑</button>

    <!-- === CARD 1: COVER === -->
    <section class="card cover-card" id="card-1">
        <!-- 卡片内容 -->
    </section>

    <!-- === CARD 2-N: CONTENT === -->
    <section class="card content-card" id="card-2">
        <!-- 卡片内容 -->
    </section>

    <!-- === CARD N: CLOSING === -->
    <section class="card closing-card" id="card-last">
        <!-- 卡片内容 -->
    </section>

    <script>
        /* === CARD CONTROLLER === */
        /* === INLINE EDITOR（可选）=== */
    </script>
</body>
</html>
```

---

## 必须包含的 JavaScript 功能

### 1. 卡片控制器

```javascript
class CardPresentation {
    constructor() {
        this.cards = Array.from(document.querySelectorAll('.card'));
        this.total = this.cards.length;
        this.current = 0;
        this.setupIntersectionObserver(); // 动效触发
        this.setupKeyboardNav();          // 方向键导航
        this.setupTouchNav();             // 触控滑动
        this.setupWheelNav();             // 滚轮导航
        this.setupProgressBar();
        this.setupNavDots();
        this.setupCardCounter();
        this.cards[0].classList.add('visible'); // 初始化第一张
    }

    setupIntersectionObserver() {
        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.classList.add('visible');
                    this.current = this.cards.indexOf(entry.target);
                    this.updateUI();
                }
            });
        }, { threshold: 0.4 });
        this.cards.forEach(card => observer.observe(card));
    }

    scrollTo(index) {
        if (index < 0 || index >= this.total) return;
        this.cards[index].scrollIntoView({ behavior: 'smooth' });
    }

    setupKeyboardNav() {
        document.addEventListener('keydown', (e) => {
            if (e.target.getAttribute('contenteditable')) return;
            if (['ArrowDown','ArrowRight',' '].includes(e.key)) {
                e.preventDefault(); this.scrollTo(this.current + 1);
            }
            if (['ArrowUp','ArrowLeft'].includes(e.key)) {
                e.preventDefault(); this.scrollTo(this.current - 1);
            }
        });
    }

    setupTouchNav() {
        let startY = 0;
        document.addEventListener('touchstart', e => { startY = e.touches[0].clientY; }, {passive:true});
        document.addEventListener('touchend', e => {
            if (e.target.getAttribute('contenteditable')) return;
            const delta = startY - e.changedTouches[0].clientY;
            if (Math.abs(delta) > 50) this.scrollTo(this.current + (delta > 0 ? 1 : -1));
        }, {passive:true});
    }

    setupWheelNav() {
        let last = 0;
        document.addEventListener('wheel', e => {
            const now = Date.now();
            if (now - last < 800) return;
            last = now;
            this.scrollTo(this.current + (e.deltaY > 30 ? 1 : e.deltaY < -30 ? -1 : 0));
        }, {passive:true});
    }

    updateUI() {
        this.updateProgress();
        this.updateNavDots();
        this.updateCardCounter();
    }

    setupProgressBar() { this.updateProgress(); }
    updateProgress() {
        const bar = document.getElementById('progressBar');
        if (bar) bar.style.width = ((this.current + 1) / this.total * 100) + '%';
    }

    setupNavDots() {
        const nav = document.getElementById('navDots');
        if (!nav) return;
        nav.innerHTML = this.cards.map((_, i) =>
            `<button class="nav-dot${i===0?' active':''}" onclick="presentation.scrollTo(${i})" aria-label="卡片${i+1}"></button>`
        ).join('');
    }
    updateNavDots() {
        document.querySelectorAll('.nav-dot').forEach((d,i) =>
            d.classList.toggle('active', i === this.current));
    }

    setupCardCounter() { this.updateCardCounter(); }
    updateCardCounter() {
        const el = document.getElementById('cardCounter');
        if (el) el.textContent = `${this.current + 1} / ${this.total}`;
    }
}

const presentation = new CardPresentation();
```

### 2. UI 元素 CSS

```css
/* === PROGRESS BAR === */
.progress-bar {
    position: fixed; top: 0; left: 0;
    height: 3px;
    background: linear-gradient(to right, var(--accent), var(--accent-secondary, var(--accent)));
    width: 0%; z-index: 1000;
    transition: width 0.3s ease;
}

/* === NAV DOTS（右侧）=== */
.nav-dots {
    position: fixed; right: clamp(0.6rem, 2vw, 1.2rem); top: 50%;
    transform: translateY(-50%);
    display: flex; flex-direction: column; gap: 0.4rem; z-index: 100;
}
.nav-dot {
    width: clamp(5px, 1.2vw, 8px); height: clamp(5px, 1.2vw, 8px);
    border-radius: 50%; border: none; padding: 0; cursor: pointer;
    background: rgba(255,255,255,0.25); transition: all 0.3s ease;
}
.nav-dot.active { background: var(--accent); transform: scale(1.4); }

/* === CARD COUNTER（底部中央）=== */
.card-counter {
    position: fixed; bottom: clamp(0.6rem, 2vw, 1rem); left: 50%;
    transform: translateX(-50%);
    font-size: clamp(0.55rem, 1.8vw, 0.8rem);
    color: rgba(255,255,255,0.4); z-index: 100;
    font-family: var(--font-body, sans-serif); letter-spacing: 0.1em;
    pointer-events: none;
}
```

### 3. 内联编辑（仅用户选择"是"时生成）

参照 frontend-slides 中的 InlineEditor 实现：
- JS hover + 400ms 延迟显示编辑按钮（不用 CSS `~` sibling）
- `E` 键切换编辑模式
- `Ctrl+S` 导出修改后 HTML
- localStorage 自动保存

---

## 代码质量要求

- 每个 section 用 `/* === SECTION NAME === */` 注释块
- 所有颜色/字体通过 `:root` CSS 变量定义
- 字体只用 Google Fonts 或 Fontshare（绝不用系统字体）
- 有意义的 `aria-label` 属性
- 键盘导航完整可用

---

## 导出为图片（告知用户）

生成 HTML 后告知用户以下导出方式：

1. **浏览器截图**：Chrome DevTools → Cmd+Shift+P → "Capture node screenshot"，选中每张 `.card`
2. **全页截图插件**：GoFullPage（Chrome）/ FireShot
3. **命令行**：`node -e "..."` 配合 Puppeteer（高级用户）
4. **直接使用 HTML**：部分平台支持 HTML 文件，或用于本地存档

推荐截图尺寸：**1080×1440px**（主流小红书封面尺寸）
