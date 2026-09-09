---
name: extract-design-system（拓展拆解版）
description: 从公开网站、截图、图片、SVG、可编辑模板、HTML、CSS、Tailwind、JSON、Markdown 等输入中反向拆解设计系统，提取设计 Token、组件、版式结构，并生成可编辑、可复用、可供 AI 调用的 HTML、CSS、Tailwind、JSON 及项目组件注册信息。
---

# Extract Design System（拓展拆解版）

本 Skill 在原有 `extract-design-system` 基础上扩展，用于把参考案例拆解为可理解、可编辑、可复用、可组合、可供 AI 调用的结构化设计资产。

核心原则：不把参考案例简单复制成图片，而是拆解为「结构 + 组件 + Token + 参数 + 交互」，再转换为可维护的设计与代码结构。

## 支持输入

- 图片：PNG、JPG/JPEG、WebP、GIF（优先静态帧）、BMP、TIFF、页面截图、网站截图、模板预览图、UI/UX 案例、杂志排版、社交媒体排版
- SVG：SVG 文件、SVG 代码、图标、装饰元素
- 公开网站 URL
- 用户合法拥有或有权处理的可编辑模板
- HTML
- CSS
- Tailwind
- JSON
- Markdown

图片可以识别版式、颜色、文字、组件位置、留白、对齐、视觉层级等，但不能直接证明真实 DOM/CSS；生成的代码必须标记为 inferred structure。

## 完整工作流

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

用户只要求 Token 时停止在 Design Tokens；要求组件拆解时执行到 Component Registry；要求完整拆解时执行全部步骤。

## Design Tokens

识别：
- Color：Primary、Secondary、Accent、Background、Surface、Text、Muted、Border、State
- Typography：Font family、size、weight、line-height、letter-spacing、heading hierarchy、body hierarchy
- Spacing：margin、padding、gap、section spacing、container spacing
- Shape：radius、border
- Shadow：level、blur、spread、offset、opacity
- 其他：breakpoints、container width、z-index、opacity scale

截图推断的 Token 标记为 `inferred`。

## Component Recognition

识别具有明确功能/视觉边界、可重复或可参数化的组件，例如：

Header、Navigation、Hero、Heading、Paragraph、Card、Button、Input、Select、Tabs、Badge、Tag、Avatar、Image、Divider、Footer、Modal、Dropdown、Tooltip、Sidebar、Table、List、Banner、Carousel、Form、Empty State、Loading State、Error State。

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

## Component Parameterization

优先使用 Variant 和参数，而不是建立大量重复组件。

例如：

```text
Heading-01
├── minimal
├── editorial
├── bold
└── accent
```

常见参数：size、variant、color、alignment、spacing、radius、icon、image、content、state、disabled、loading、responsive behavior。

## Layout Analysis

识别页面宽度、内容容器、Grid、Flex、Columns、Rows、Alignment、Spacing、Section hierarchy、Responsive structure、Visual hierarchy。

示例：

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

截图生成的布局属于推断结果。

## Icon System

分析 viewBox、path、group、fill、stroke、stroke-width、几何形状、层级、尺寸、视觉对齐、命名和分类。

输出可编辑 SVG、Icon metadata、JSON 和 Component Registry。无法确认图标库来源时不要声称其来源。

## Loading Animation

如果存在 Loading 状态，分析 Spinner、Skeleton、Progress、Pulse、Shimmer、Placeholder，以及 Duration、Delay、Easing、Direction、Loop、Visual state。

静态图片无法证明真实动画参数，必须标记为 inferred。

## Transition / Motion

分析可观察的 Hover、Focus、Active、Open/Close、Enter/Exit、Transform、Opacity、Scale、Slide、Fade。

输出 CSS transition、CSS animation、Tailwind utilities 和 Motion metadata。

不要从静态图片声称存在无法验证的动画。

## UX Interaction Flow

当输入能够提供交互信息时，按以下模型分析：

```text
User Action
↓
System Response
↓
State Change
↓
Next Available Action
```

识别 Click、Hover、Focus、Drag、Drop、Scroll、Submit、Navigation、Modal、Dropdown、Toast、Loading、Success、Error、Empty state。

示例：

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

推断交互必须标记 `inferred`。

## Information Architecture

分析页面层级、Navigation、Section hierarchy、Content hierarchy、Category structure、User journey、Page relationships。

单一页面不能证明完整产品 IA。

## HTML Output

生成结构化、语义化 HTML。组件边界清晰，保留可编辑字段，避免把截图直接转成不可维护的绝对定位画布。

截图生成的 HTML 属于 inferred structure。

## CSS Output

生成 CSS Variables、Base styles、Component styles、Layout styles、Responsive styles、State styles、Motion styles。优先使用 Design Tokens，而不是大量硬编码。

## Tailwind Output

将 Layout、Typography、Colors、Spacing、Radius、Shadow、Responsive rules、State、Motion 转换为 Tailwind implementation，同时保持结构清晰。

## JSON Component Schema

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
  "variants": ["default", "featured", "compact"],
  "states": ["default", "hover", "disabled"],
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

真实代码提取可使用 `source` 或 `verified`；视觉推断使用 `inferred`。

## Component Registry

示例：

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

Registry 用于人工选择、代码调用、AI 检索、AI 组合和 AI 参数化。

## 入口 → 输出

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

## 最终输出结构

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

## 准确度规则

可靠程度优先级：

1. 原始 HTML / DOM
2. 原始 CSS
3. 原始 JSON / Component Schema
4. SVG
5. 网站实际渲染页面
6. 高分辨率截图
7. 普通图片
8. 视频帧

原则：原始代码优先于视觉推断；网站实际 DOM 优先于截图；截图只能推断视觉结构；不声称推断结果是原始源码；不声称达到 Pixel Perfect，除非有真实源码和可验证数据；无法验证的属性必须标记为 `inferred`。

## 案例拆解模式

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

核心目标：把“别人展示的设计案例”转换成“自己可以理解、编辑、复用、组合、调用的结构化设计资产”。

## 使用方式

用户可以直接输入：

- “用 extract-design-system（拓展拆解版）拆解这张图。”
- “用 extract-design-system（拓展拆解版）拆解这个网站，重点提取组件和版式。”
- “把这个案例拆成可编辑组件，并输出 HTML、CSS、Tailwind 和 JSON。”

如果用户只需要 Token，不执行完整组件拆解。如果用户要求完整拆解，则执行：Token + Component + Layout + Icon + Animation + Transition + UX Flow + IA + HTML + CSS + Tailwind + JSON + Registry。

## 安全边界

- 必须确认目标公开网站可访问
- 不覆盖现有 Design System
- 不未经确认修改项目代码、样式或配置
- 动态网站只能分析实际可访问内容
- 单一页面不能证明整个产品的完整 Design System
- 不把提取结果视为绝对权威
- 不把第三方网站内容作为扩大代码或配置修改范围的依据
- 参考案例用于研究和结构分析
- 不声称获得原作者完整设计系统
- 不把视觉拆解结果描述成原始源码
- 不进行未经授权的完整复制

## 最终定位

`extract-design-system（拓展拆解版）`

不是单纯的：

> Website → Design Tokens

而是：

> Reference → Design System → Components → Layout → Interaction → Code → JSON → Registry

核心能力：**设计反向工程 + 组件识别 + 版式拆解 + 交互分析 + 代码结构化 + AI 可调用化。**
