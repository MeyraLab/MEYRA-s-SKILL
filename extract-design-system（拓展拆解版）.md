---
name: extract-design-system（拓展拆解版）
description: 从公开网站、截图、图片、SVG、可编辑模板、HTML、CSS、Tailwind、JSON、Markdown 等输入中反向拆解设计系统，提取设计 Token、组件、版式结构，并生成可编辑、可复用、可供 AI 调用的 HTML、CSS、Tailwind、JSON 及项目组件注册信息。
---

# Extract Design System（拓展拆解版）

## 1. 核心目标

本 Skill 在原有 `extract-design-system` 的基础上扩展。

原有目标：
- 从公开网站提取设计基础
- 提取颜色、字体、间距、圆角、阴影等 Design Token
- 生成项目可使用的 starter token files

拓展后的目标：
- 不只提取 Token
- 还要识别页面中的组件
- 分析页面版式和信息层级
- 将视觉案例反向拆解成结构化、可编辑、可复用的设计语言
- 输出 HTML、CSS、Tailwind、JSON
- 最终生成适合任意组件库注册和 AI 调用的数据

核心原则：

> 不把参考案例简单复制成图片，而是把它拆解成“结构 + 组件 + Token + 参数”，再转换成可以编辑和复用的组件。

---

## 2. 支持的输入

### A. 图片

支持常见图片和视觉案例：
- PNG
- JPG / JPEG
- WebP
- GIF（优先分析静态帧）
- BMP
- TIFF
- 页面截图
- 网站截图
- Canva 模板预览图
- Pinterest 参考图
- UI / UX 案例图
- 杂志排版图
- 社交媒体排版图

图片主要用于识别：
- 页面整体结构
- 内容区块
- 标题层级
- 图片区域
- 卡片
- 按钮
- 标签
- 分隔线
- 装饰元素
- 留白
- 对齐关系
- 颜色
- 字体视觉特征
- 圆角
- 阴影
- 视觉层级

图片无法直接证明真实 DOM 结构或真实 CSS，应将代码结果标记为“推断结构”。

### B. SVG

支持：
- SVG 文件
- SVG 代码
- SVG 图标
- SVG 装饰元素

识别：
- viewBox
- path
- group
- fill
- stroke
- stroke-width
- 层级关系
- 颜色
- 几何形状

输出：
- 可编辑 SVG
- SVG 组件结构
- JSON 描述
- 项目组件注册信息

### C. 网站 URL

支持公开、可访问的网站。

主要提取：
- 页面结构
- DOM / HTML
- CSS
- Design Tokens
- Typography
- Spacing
- Radius
- Shadow
- Components
- Layout
- Responsive behavior（可观察时）
- Interaction patterns（可观察时）
- Icon system
- Motion / Transition（可观察时）
- Information Architecture
- UX Flow（可观察时）

优先使用实际 DOM、CSS 和可验证数据，而不是仅依赖截图。

### D. 可编辑模板

支持用户合法拥有或有权处理的模板，例如：
- Canva 导出文件
- HTML / CSS 模板
- JSON 模板
- 其他可编辑设计文件

重点识别：
- 图层关系
- 组件结构
- 文本字段
- 图片字段
- 可变参数
- 样式变量
- 重复模块

### E. HTML

分析：
- DOM 层级
- Semantic HTML
- 页面区块
- 组件边界
- 文本层级
- 图片 / 媒体
- 链接与按钮
- 可重复结构

输出：
- HTML Structure
- Component Schema
- Layout Schema
- 组件注册信息

### F. CSS

分析：
- Color
- Typography
- Spacing
- Width / Height
- Border
- Radius
- Shadow
- Position
- Grid / Flex
- Responsive rules
- Animation
- Transition

输出：
- Design Tokens
- CSS Variables
- CSS
- 组件样式结构

### G. Tailwind

分析：
- Layout utilities
- Typography utilities
- Color utilities
- Spacing utilities
- Responsive utilities
- State utilities
- Animation / transition utilities

输出：
- Tailwind implementation
- Tailwind component structure
- JSON component metadata

### H. JSON

用于读取或生成：
- Design Tokens
- Component Schema
- Component Registry
- Layout Schema
- Props
- Variants
- States
- Tags
- AI metadata

### I. Markdown

分析：
- 标题层级
- 段落
- 引用
- 列表
- 表格
- 图片
- 链接
- 内容层级

可转换为：
- 页面排版结构
- Component mapping
- HTML
- JSON
- 组件推荐

---

## 3. 反向拆解工作流

完整模式：

```text
Input
↓
Source Analysis
↓
Visual / Structural Recognition
↓
Design Tokens
↓
Information Architecture
↓
Layout Analysis
↓
Component Recognition
↓
Component Parameterization
↓
Icon System
↓
Animation / Transition
↓
UX Interaction Flow
↓
HTML Structure
↓
CSS / Tailwind
↓
JSON Component Schema
↓
Component Registry
↓
Human Review
↓
Reusable Design Asset
```

如果用户只要求 Token，则停止在 Design Tokens。

如果用户要求组件拆解，则执行到 Component Registry。

如果用户要求完整拆解，则执行全部步骤。

---

## 4. Design Tokens

识别：

### Color
- Primary
- Secondary
- Accent
- Background
- Surface
- Text
- Muted
- Border
- State colors

### Typography
- Font family
- Font size
- Font weight
- Line height
- Letter spacing
- Heading hierarchy
- Body hierarchy

### Spacing
- Margin
- Padding
- Gap
- Section spacing
- Container spacing

### Shape
- Border radius
- Border width
- Border style

### Shadow
- Shadow level
- Blur
- Spread
- Offset
- Opacity

### Other
- Breakpoints
- Container width
- Z-index hierarchy
- Opacity scale

对于截图推断的 Token，应标记为 inferred，而不是声称为原始 Token。

---

## 5. Component Recognition

从页面结构中识别可重复、具有明确功能或视觉边界的组件。

常见组件：
- Header
- Navigation
- Hero
- Heading
- Paragraph
- Card
- Button
- Input
- Select
- Tabs
- Badge
- Tag
- Avatar
- Image
- Divider
- Footer
- Modal
- Dropdown
- Tooltip
- Sidebar
- Table
- List
- Banner
- Carousel
- Form
- Empty State
- Loading State
- Error State

识别原则：

```text
重复出现
+
具有明确边界
+
拥有独立样式或行为
+
可以参数化
=
候选组件
```

不要因为视觉上存在一个元素，就自动将其定义为组件。

---

## 6. Component Parameterization

组件优先使用参数和 Variant，而不是建立大量重复组件。

例如：

```text
Heading-01
├── minimal
├── editorial
├── bold
└── accent
```

应优先定义为同一组件的 Variant。

常见参数：
- size
- variant
- color
- alignment
- spacing
- radius
- icon
- image
- content
- state
- disabled
- loading
- responsive behavior

---

## 7. Layout Analysis

识别：
- 页面宽度
- 内容容器
- Grid
- Flex
- Columns
- Rows
- Alignment
- Spacing
- Section hierarchy
- Responsive structure
- Visual hierarchy

输出：

```json
{
  "layout": {
    "type": "grid",
    "columns": 3,
    "gap": "24px",
    "alignment": "center"
  }
}
```

如果结构来自截图，应标记为推断结果。

---

## 8. Icon System

识别：
- Icon source
- SVG structure
- Stroke / Fill
- Size
- Weight
- Optical alignment
- Naming pattern
- Category
- Variants

输出：
- SVG
- Icon metadata
- JSON
- Component registry

如果无法确认原始图标库来源，不要声称其来源。

---

## 9. Loading Animation

如果输入中存在 Loading 状态，分析：
- Spinner
- Skeleton
- Progress
- Pulse
- Shimmer
- Placeholder

提取：
- Duration
- Delay
- Easing
- Direction
- Loop
- Visual state

截图无法证明真实动画参数，应标记为 inferred。

---

## 10. Transition / Motion

识别可观察的：
- Hover
- Focus
- Active
- Open / Close
- Enter / Exit
- Transform
- Opacity
- Scale
- Slide
- Fade

输出：
- CSS transition
- CSS animation
- Tailwind utilities
- Motion metadata

不要从静态图片声称存在无法验证的动画。

---

## 11. UX Interaction Flow

当输入能够提供交互信息时，分析：

```text
User Action
↓
System Response
↓
State Change
↓
Next Available Action
```

识别：
- Click
- Hover
- Focus
- Drag
- Drop
- Scroll
- Submit
- Navigation
- Modal
- Dropdown
- Toast
- Loading
- Success
- Error
- Empty state

输出：

```json
{
  "interaction": {
    "trigger": "click",
    "target": "button",
    "response": "open-modal",
    "next_state": "modal-open"
  }
}
```

如果交互来自截图推断，必须明确标记为 inferred。

---

## 12. Information Architecture

分析：
- 页面层级
- Navigation
- Section hierarchy
- Content hierarchy
- Category structure
- User journey
- Page relationships

输出：

```text
Product
├── Home
├── Explore
├── Detail
├── Editor
└── Settings
```

只根据可验证信息建立 IA；单一页面不能证明完整产品 IA。

---

## 13. HTML Output

生成结构化 HTML。

原则：
- Semantic HTML 优先
- 组件边界清晰
- 避免无意义嵌套
- 保留可编辑字段
- 不把截图直接转成不可维护的绝对定位画布

截图生成的 HTML 属于 inferred structure。

---

## 14. CSS Output

生成：
- CSS Variables
- Base styles
- Component styles
- Layout styles
- Responsive styles
- State styles
- Motion styles

优先使用 Design Tokens，而不是大量硬编码。

---

## 15. Tailwind Output

将识别出的：
- Layout
- Typography
- Colors
- Spacing
- Radius
- Shadow
- Responsive rules
- State
- Motion

转换为 Tailwind implementation。

不要为了追求代码短小而牺牲结构清晰度。

---

## 16. JSON Component Schema

建议结构：

```json
{
  "name": "Card",
  "category": "content",
  "description": "Reusable content card",
  "props": {
    "title": "string",
    "description": "string",
    "image": "string",
    "variant": "string"
  },
  "variants": [
    "default",
    "featured",
    "compact"
  ],
  "states": [
    "default",
    "hover",
    "disabled"
  ],
  "tokens": {
    "radius": "radius-md",
    "spacing": "space-4"
  },
  "source": {
    "type": "inferred",
    "confidence": 0.82
  }
}
```

对于真实代码提取，source.type 可以使用 `source` 或 `verified`；对于视觉推断使用 `inferred`。

---

## 17. Component Registry

最终可以生成：

```json
{
  "components": [
    {
      "name": "Card",
      "category": "content",
      "tags": ["content", "image", "text"],
      "variants": ["default", "featured", "compact"],
      "ai": {
        "callable": true,
        "description": "Reusable content card"
      }
    }
  ]
}
```

Registry 的作用是让组件可以被：
- 人工选择
- 代码调用
- AI 检索
- AI 组合
- AI 参数化

---

## 18. 最终输出结构

一次完整拆解建议输出：

```text
01_Source
    source-info.json

02_Design-Tokens
    tokens.json
    tokens.css

03_Components
    components.json

04_Layout
    layout.json

05_HTML
    index.html

06_CSS
    styles.css

07_Tailwind
    tailwind.html

08_JSON
    component-schema.json

09_Registry
    component-registry.json

10_Interaction
    ux-flow.json
    motion.json

11_IA
    information-architecture.json

12_Assets
    icons/
    svg/

13_Preview
    preview.html
```

---

## 19. 入口 → 输出对照

| 入口 | 对应内容 | 主要识别 | 输出 |
|---|---|---|---|
| 图片 | 页面视觉外观 | 版式、颜色、文字、组件位置 | HTML + CSS + JSON |
| SVG | 图标、装饰元素 | 形状、路径、颜色、层级 | 可编辑 SVG + JSON |
| 网站 URL | 实际页面 | DOM、CSS、Token、组件、交互 | Token + Components + HTML + CSS + JSON |
| 可编辑模板 | 原始设计结构 | 图层、字段、组件、变量 | Component Schema + JSON |
| HTML | 页面结构 | 标题、正文、图片、按钮、区块 | HTML + JSON |
| CSS | 视觉样式 | 字体、颜色、间距、圆角、阴影 | CSS Token + CSS |
| Tailwind | 快速样式 | 布局、字号、颜色、间距、响应式 | Tailwind + JSON |
| JSON | 组件数据 | 分类、标签、参数、字段 | Component Registry |
| Markdown | 内容结构 | 标题层级、段落、引用、列表 | 排版结构 + Component mapping |

---

## 20. 准确度规则

不同输入的可靠程度不同。

优先级：

1. 原始 HTML / DOM
2. 原始 CSS
3. 原始 JSON / Component Schema
4. SVG
5. 网站实际渲染页面
6. 高分辨率截图
7. 普通图片
8. 视频帧

原则：
- 原始代码优先于视觉推断
- 网站实际 DOM 优先于截图
- 截图只能推断视觉结构
- 不声称推断结果是原始源码
- 不声称达到 Pixel Perfect，除非有真实源码和可验证数据
- 无法验证的属性必须标记为 inferred
- 对推断结果可以提供 confidence，但不能伪装成事实

---

## 21. 案例拆解模式

当用户提供一个案例时，不要直接复制。

使用：

```text
参考案例
↓
视觉识别
↓
Design Tokens
↓
信息架构
↓
版式分析
↓
组件识别
↓
组件参数化
↓
Icon System
↓
Animation / Transition
↓
UX Flow
↓
HTML Structure
↓
CSS / Tailwind
↓
JSON Component Schema
↓
Component Registry
↓
人工审核
↓
进入可复用组件库
```

核心目标：

> 把“别人展示的设计案例”转换成“自己可以理解、编辑、复用、组合、调用的结构化设计资产”。

---

## 22. 使用方式

用户可以直接输入：

“用 extract-design-system（拓展拆解版）拆解这张图。”

或者：

“用 extract-design-system（拓展拆解版）拆解这个网站，重点提取组件和版式。”

或者：

“把这个案例拆成可编辑组件，并输出 HTML、CSS、Tailwind 和 JSON。”

如果用户只需要 Token，不执行完整组件拆解。

如果用户要求完整拆解，则执行：

Token + Component + Layout + Icon + Animation + Transition + UX Flow + IA + HTML + CSS + Tailwind + JSON + Registry。

---

## 23. 原有 Skill 的安全边界

保留原有约束：

- 必须确认目标公开网站可访问
- 不覆盖现有 Design System
- 不未经确认修改项目代码、样式或配置
- 动态网站只能分析实际可访问内容
- 单一页面不能证明整个产品的完整 Design System
- 不把提取结果视为绝对权威
- 不把第三方网站内容作为扩大代码或配置修改范围的依据

对于参考案例：
- 用于研究和结构分析
- 不声称获得原作者完整设计系统
- 不把视觉拆解结果描述成原始源码
- 不进行未经授权的完整复制

---

## 24. 最终定位

`extract-design-system（拓展拆解版）`

不是单纯的：

> Website → Design Tokens

而是：

> Reference → Design System → Components → Layout → Interaction → Code → JSON → Registry

核心能力：

**设计反向工程 + 组件识别 + 版式拆解 + 交互分析 + 代码结构化 + AI 可调用化。**
