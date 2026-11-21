# UI Style Specification — Soft UI / Neumorphism

> 你是一名资深前端工程师 + 视觉设计师。请严格按照以下规范实现界面，任何偏离都视为缺陷。
> 风格代号：`Soft UI / Neumorphism`（柔和拟物）
> 一句话定义：同一底色上双向外阴影与内阴影，控件像从界面里被“压出来”。

---

## 0. 硬性约束（最高优先级）

1. 技术栈：React + TypeScript + Tailwind CSS（无额外 UI 库；如需组件库，只能用 headless 方案）。
2. 本规范中的 Tailwind 类名是**契约**，不是建议——必须在代码中原样出现或等价实现。
3. 不得引入未在色板中定义的颜色；不得引入第二套圆角/阴影体系。
4. 先实现静态视觉，再实现交互；交互反馈必须符合「组件规范」中的三态描述。
5. 如遇规范未覆盖的场景，选择**更克制、更贴近现有 token** 的方案，并在注释中说明理由。

---

## 1. 风格基因（Style DNA）

| 维度 | 规定 |
| --- | --- |
| 视觉印象 | 同一底色上双向外阴影与内阴影，控件像从界面里被“压出来”。 |
| 核心手段 | 双向阴影 / 同色系 / 大圆角 / 柔和 |
| 布局密度 | 均衡 |
| 情绪关键词 | 双向阴影、同色系、大圆角、柔和 |

## 2. Design Tokens

### 2.1 色板（唯一允许使用的颜色）

| 名称 | 值 | 用途 |
| --- | --- | --- |
| Base | `#F3EEF8` | 页面背景 & 控件底色 |
| Light | `#FFFFFF` | 拟物高光阴影 |
| Dark | `#DCD2E8` | 拟物暗部阴影 |
| Accent | `#6C5CE7` | 强调 / 链接 / 选中态 |
| Text | `#3D3350` | 主文本 |
| Muted | `#8A7D9E` | 辅助文字 / 占位符 |

### 2.2 Tailwind 类名契约

- **底色** → `bg-[#F3EEF8]（凸起与凹陷同底）`
- **外阴影(凸)** → `shadow-[8px_8px_16px_#DCD2E8,-8px_-8px_16px_#FFFFFF]`
- **内阴影(凹)** → `shadow-[inset_2px_2px_5px_#DCD2E8,inset_-2px_-2px_5px_#FFF]`
- **圆角** → `rounded-2xl (16px) / rounded-3xl (24px)`
- **主色** → `bg-[#6C5CE7] text-white`
- **边框** → `border-0（靠阴影分界，永不描边）`
- **字色** → `text-[#3D3350] / text-[#8A7D9E]`

### 2.3 tailwind.config 扩展

```js
// tailwind.config.js — theme.extend
colors: { soft: { base: '#F3EEF8', light: '#FFFFFF', dark: '#DCD2E8', accent: '#6C5CE7' } },
boxShadow: {
  soft: '8px 8px 16px #DCD2E8, -8px -8px 16px #FFFFFF',
  'soft-sm': '4px 4px 9px #DCD2E8, -4px -4px 9px #FFFFFF',
  'soft-inset': 'inset 2px 2px 5px #DCD2E8, inset -2px -2px 5px #FFFFFF',
},
```

## 3. 组件规范（Component Rules）

### 3.1 Button
圆角 12–16px，渐变填充 linear-gradient(145deg, 亮主色, 暗主色) + 双向外阴影；hover 时阴影减弱（模拟被按亮），active 时切换为 inset 阴影 —— 这是拟物的核心反馈。

### 3.2 Card
bg-[#F3EEF8] rounded-3xl shadow-[8px_8px_16px_#DCD2E8,-8px_-8px_16px_#FFF] p-6，无边框无描边，卡片与背景同色，仅靠阴影“凸起”。

### 3.3 Input
凹陷态：bg-[#F3EEF8] + inset 双向阴影，圆角 12px，placeholder 用 #B3A8C4；focus 时 inset 加深并加 1px 主色内描边。

### 3.4 Toggle / Slider
凹槽 + 凸起圆钮，圆钮用渐变 + 外阴影，切换时圆钮平滑平移 200ms。

### 3.5 Typography
偏中等字重（500–600），字色不用纯黑，用带紫调的深灰 #3D3350；正文 14–15px，行高 1.6。

### 3.6 间距
padding 20–28px，圆角与 padding 比例协调；元素间距 16–20px，避免拥挤。

## 4. 必须做到（Do）

- 凸起和凹陷的两个阴影必须来自同一底色的亮/暗两档
- 所有控件圆角 ≥ 12px，且整站统一
- 交互反馈用「阴影翻转」（外 → 内）而不是颜色变化
- 主色只用于 CTA，其余全部同色系

## 5. 禁止事项（Don'ts — 违反即为不合格）

- ❌ 禁止 1px 边框描边——拟物不能用线分界
- ❌ 禁止纯黑阴影、禁止高对比硬阴影
- ❌ 禁止在彩色背景上做拟物（会失去同色阴影逻辑）
- ❌ 禁止同时出现玻璃拟态或粗野主义元素
- ❌ 禁止使用 font-weight 700+ 的粗黑标题

## 6. 执行清单

- [ ] 页面中不存在色板之外的颜色
- [ ] 圆角/边框/阴影全部来自 tokens 表，无临时值
- [ ] 每个可交互元素都有 hover / focus-visible / active 三态
- [ ] 已逐条核对「禁止事项」，无一条被违反
- [ ] 在 375px / 768px / 1280px 三个断点下视觉一致

---

<!-- 把这段规范完整粘贴给 AI 编码助手，然后在同一轮对话里提出具体页面需求。 -->
