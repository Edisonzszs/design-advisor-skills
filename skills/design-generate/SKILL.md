---
name: design-generate
description: "设计输出生成 - 生成标准格式的 DESIGN.md 文件，支持定制微调。"
---

# Design Generate - 输出生成

本 skill 负责将选定或混合后的设计 token 输出为标准 DESIGN.md 格式文件。

## 输入

两种输入模式：

**模式 A：单品牌定制**
- 品牌名
- 用户需求参数
- 需要微调的项目（可选）

**模式 B：混合方案**
- 混合后的 token 集合（来自 design-hybrid）
- 用户需求参数

## 输出流程

### Step 1: 读取源 DESIGN.md

**模式 A：** 读取 `design-md/{brand}/DESIGN.md` 的完整内容
**模式 B：** 使用 design-hybrid 输出的混合 token 集合

### Step 2: 需求微调

根据用户需求对 token 进行定制调整（仅模式 A 或用户要求时）：

**常见微调项：**
- 替换主色为用户品牌色
- 调整字体为开源替代（如 SF Pro → Inter）
- 增加/减少组件
- 调整间距密度
- 增加暗色模式色彩

**微调原则：**
- 只调整用户明确要求的项
- 保持原始设计系统的核心特征（如 Apple 的极简、Sentry 的双极性）
- 所有调整后的值必须保持 token 引用一致性

### Step 3: 生成 YAML Front Matter

按标准格式生成：

```yaml
---
version: alpha
name: "{项目名}-design-system"
description: "{基于 {Brand} 风格定制，2-4 句话描述设计语言}"

colors:
  primary: "#..."
  on-primary: "#..."
  ink: "#..."
  canvas: "#..."
  # ... 完整色彩定义

typography:
  hero-display:
    fontFamily: "..."
    fontSize: "..."
    fontWeight: ...
    lineHeight: "..."
    letterSpacing: "..."
  # ... 完整字体层级

rounded:
  sm: "...px"
  md: "...px"
  lg: "...px"
  pill: "9999px"

spacing:
  xxs: "...px"
  xs: "...px"
  sm: "...px"
  md: "...px"
  lg: "...px"
  xl: "...px"
  section: "...px"

components:
  button-primary:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.on-primary}"
    typography: "{typography.button-md}"
    rounded: "{rounded.pill}"
    padding: "..."
  # ... 完整组件定义
---
```

### Step 4: 生成 Markdown Body

按标准 9 节格式生成：

1. **Overview** — 设计哲学、核心特征、关键特性列表
2. **Colors** — 色彩语义说明（品牌色、表面色、文字色、分割线色、语义色）
3. **Typography Rules** — 字体家族、层级表、排版原则、字体替代说明
4. **Layout** — 间距系统、网格与容器、留白哲学
5. **Elevation & Depth** — 阴影层级表、装饰深度
6. **Shapes** — 圆角层级表、图形几何
7. **Components** — 每个组件的详细规格（按钮、卡片、输入框、导航等）
8. **Do's and Don'ts** — 设计守则和反模式
9. **Responsive Behavior** — 断点、触控目标、折叠策略

### Step 5: 输出文件

**判断逻辑：调用方会告诉你本次是什么操作类型。**

#### 操作类型：纯引用

不做任何文件操作。只返回原文件路径：
```
已选择 {Brand} 风格。
原始 DESIGN.md: {插件目录}/design-md/{brand}/DESIGN.md
```

#### 操作类型：单源微调 / 多源混合

将生成的 DESIGN.md 写入**用户当前工作目录（CWD）**。

- 输出路径: `{CWD}/DESIGN.md`
- 如果 CWD 下已有 DESIGN.md，提示用户是否覆盖

生成完成后输出简短说明：

```markdown
## DESIGN.md 已生成

**文件路径:** `{CWD}/DESIGN.md`
**操作类型:** {单源微调 基于 {Brand} / 多源混合 {BrandA}+{BrandB}}
**定制项:** {列出修改/混合的内容}

### 使用方法

1. 文件已在项目根目录
2. 告诉 AI 编码工具: "按照 DESIGN.md 的设计规范构建页面"

### 注意事项

- 字体: {说明字体替代方案}
- 专有资源: {说明哪些资源需要替换}
- 推荐技术栈: {根据风格推荐前端框架/组件库}
```

## 设计系统输出质量标准

1. **Token 引用一致性** — 所有 `{colors.xxx}`, `{typography.xxx}` 引用都必须在 front matter 中定义
2. **色彩可访问性** — 主色与背景色的对比度 >= 4.5:1（WCAG AA）
3. **字体回退** — 每个字体栈必须包含 system-ui 或 -apple-system 回退
4. **组件完整性** — 至少包含：按钮（primary/secondary/ghost）、卡片、输入框、导航
5. **间距一致** — 所有间距值来自 spacing token，无硬编码像素值
6. **暗色模式** — 如果需求包含暗色，必须定义对应的暗色表面 token
