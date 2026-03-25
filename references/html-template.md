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
        // ⚠️ 不要加 setupTouchNav / setupWheelNav！
        // CSS scroll-snap 已原生处理滚轮和触控滑动。
        // JS 再拦截会触发两次跳卡（snap 一次 + scrollTo 一次）。
        this.setupProgressBar();
        this.setupNavDots();
        this.setupCardCounter();
        this.cards[0].classList.add('visible'); // 初始化第一张
    }

    setupIntersectionObserver() {
        // ⚠️ IntersectionObserver 只负责触发入场动效（.visible），
        // 不用来追踪 this.current — 因为 threshold 触发时机不准，
        // 加了 margin-bottom 间距后尤其不可靠。
        // this.current 由下方 scroll 事件维护。
        const visObserver = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) entry.target.classList.add('visible');
            });
        }, { threshold: 0.3 });
        this.cards.forEach(card => visObserver.observe(card));

        // scroll 事件实时追踪：找离视口顶部最近的卡片
        const onScroll = () => {
            let closest = 0, minDist = Infinity;
            this.cards.forEach((card, i) => {
                const dist = Math.abs(card.getBoundingClientRect().top);
                if (dist < minDist) { minDist = dist; closest = i; }
            });
            if (closest !== this.current) {
                this.current = closest;
                this.updateUI();
            }
        };
        document.body.addEventListener('scroll', onScroll, { passive: true });
        window.addEventListener('scroll', onScroll, { passive: true });
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

**⚠️ 核心注意事项：不要在 HTML 里手动给每个元素加 `contenteditable`，而是用 JS `_markEditables()` 方法自动扫描并标记。**

```javascript
class InlineEditor {
  constructor() {
    this.active = false;
    this.btn = document.getElementById('editToggle');
    this.hint = document.getElementById('editHint');
    this._markEditables(); // ← 必须在绑定事件前调用
    this.btn.addEventListener('click', () => this.toggle());
    document.addEventListener('keydown', (e) => {
      if ((e.key === 'e' || e.key === 'E') && e.target.getAttribute('contenteditable') !== 'true') {
        this.toggle(); return;
      }
      if ((e.metaKey || e.ctrlKey) && e.key === 's') { e.preventDefault(); this.save(); }
    });
  }

  _markEditables() {
    // 排除装饰性元素的类名列表（按项目调整）
    const SKIP_TAGS = new Set(['SCRIPT','STYLE','NAV','BUTTON']);
    const SKIP_CLASSES = ['nav-dot','progress-bar','card-counter',
      'view-dot','layer-dot','layer-line','white-panel','white-panel-top',
      'divider-line','noise','grid-lines','ev-icon','action-num','ct-num',
      'big-q','step-badge'];
    const isSkip = el =>
      SKIP_TAGS.has(el.tagName) || SKIP_CLASSES.some(c => el.classList.contains(c));

    document.querySelectorAll('.card *').forEach(el => {
      if (isSkip(el)) return;
      // 叶子判断：直接含文本节点 + 子元素全是内联标签
      const hasText = Array.from(el.childNodes).some(n =>
        n.nodeType === Node.TEXT_NODE && n.textContent.trim());
      const inlineOnly = Array.from(el.children).every(c =>
        ['BR','SPAN','STRONG','EM'].includes(c.tagName));
      if (hasText && inlineOnly) {
        el.setAttribute('contenteditable', 'false');
        el.dataset.editable = '1'; // 用 data-editable 而非 [contenteditable] 作为选择器
      }
    });
  }

  toggle() {
    this.active = !this.active;
    document.querySelectorAll('[data-editable]').forEach(el =>
      el.setAttribute('contenteditable', this.active ? 'true' : 'false'));
    this.btn.textContent = this.active ? '✓ 完成编辑' : '✏ 编辑';
    this.btn.classList.toggle('active-edit', this.active);
    this.hint.classList.toggle('visible', this.active);
    if (!this.active) this._saveToStorage();
  }

  save() { /* 导出 HTML 文件，⌘S 触发 */ }
  _saveToStorage() { /* localStorage 草稿 */ }
}
const editor = new InlineEditor();
```

**关键规则：**
- `_markEditables()` 自动扫描 `.card` 内所有叶子文字容器，**不需要在 HTML 里手写 `contenteditable`**
- 用 `data-editable` 作为选择器，而非 `[contenteditable]`（后者在 toggle 时会误选 navDots 的 button）
- `E` 键和 `⌘S` 都在 InlineEditor 里统一注册，**CardPresentation 里不要重复绑定 E 键**
- `⌘S` 任何时候都可以导出（不限于编辑模式中）

---

## 代码质量要求

- 每个 section 用 `/* === SECTION NAME === */` 注释块
- 所有颜色/字体通过 `:root` CSS 变量定义
- 字体只用 Google Fonts 或 Fontshare（绝不用系统字体）
- 有意义的 `aria-label` 属性
- 键盘导航完整可用

---

## 导出为图片

生成 HTML 后，**不要主动截图**，统一引导用户进入 SKILL.md 的 **Phase 5** 自动导出流程：

> 告诉用户：「改完后告诉我「导出图片」，我会自动帮你批量截图保存为 PNG。」

Phase 5 会自动：
- 用本机 Chrome + Puppeteer 批量截图
- 输出 2160×2880px PNG（@2x，对应 1080×1440 逻辑尺寸）
- 截图完毕后自动打开图片文件夹

> ⚠️ 手动截图方式（DevTools / GoFullPage）仅作为 Puppeteer 不可用时的降级方案，**不作为默认推荐**。
