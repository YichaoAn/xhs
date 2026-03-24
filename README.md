# xhs-image-cards

一个 CatPaw AI Skill，帮你快速生成**小红书图文卡片组**。

## 它能做什么

- 生成严格 **3:4 比例**（1080×1440px）的竖版图文卡片
- 一组包含：封面 + 内容页 + 结尾页
- 单个 HTML 文件，可在浏览器直接预览、截图发布
- 支持**内联编辑**：在浏览器里点击文字直接改，`⌘S` 导出

## 使用流程

1. 在 CatPaw 中告诉 AI："帮我生成小红书图文"
2. AI 会引导你填写：内容素材 → 卡片数量 → 内容类型 → 是否需要编辑
3. 看 3 个风格预览，选一个你喜欢的（或混合）
4. 生成完整卡片组，在浏览器打开 → 截图 → 发布小红书

## 12 种视觉风格

来自 [frontend-slides](https://github.com/zarazhangrui/frontend-slides)，针对小红书竖版卡片完整适配。

| 风格 | 气质 | 适合内容 |
|------|------|---------|
| Bold Signal | 撞色有冲击力 | 干货/方法论 |
| Electric Studio | 高对比专业感 | 商业/职场/品牌 |
| Creative Voltage | 复古现代有冲劲 | 创意/年轻/活力 |
| Dark Botanical | 优雅沉稳有感染力 | 情绪/日记/生活 |
| Neon Cyber | 未来感科技感 | 数码/测评/工具 |
| Terminal Green | 极客开发者美学 | 技术科普/代码 |
| Notebook Tabs | 整洁编辑感 | 知识科普/笔记 |
| Pastel Geometry | 友好柔色几何 | 生活/学习/软干货 |
| Vintage Editorial | 文艺复古杂志感 | 美食/旅行/观点 |
| Split Pastel | 马卡龙双拼活泼 | 美妆/穿搭 |
| Swiss Modern | 极简精准高级感 | 职场/效率 |
| Paper & Ink | 文学编辑沉思感 | 读书笔记/深度观点 |

## 设计规范

- 所有字体用 `cqw`（容器查询宽度），桌面/手机显示一致，不溢出
- 标题强制居中，不偏侧
- 禁止侧边彩色标签耳

## 文件结构

```
SKILL.md                    # AI 技能主文件
references/
  ├── style-presets.md      # 12 种视觉风格预设
  ├── card-base.css         # 3:4 比例基础样式
  ├── animation-patterns.md # 入场动效参考
  └── html-template.md      # HTML/JS 结构规范
```
