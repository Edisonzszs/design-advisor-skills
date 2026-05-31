---
name: design-hybrid
description: "设计混合生成 - 从多个设计系统中提取 token 进行混合，生成混合 DESIGN.md。"
---

# Design Hybrid - 混合生成

本 skill 从用户选定的 2-3 个候选品牌中混合 token，生成融合的设计系统。

## 输入

- 用户选择的 2-3 个候选品牌名
- 用户的需求参数（来自 design-advisor）

## 混合流程

### Step 1: 加载原始 DESIGN.md

读取所有选定品牌的完整 DESIGN.md：
- `design-md/{brand-a}/DESIGN.md`
- `design-md/{brand-b}/DESIGN.md`
- （可选）`design-md/{brand-c}/DESIGN.md`

解析每个文件的 YAML front matter，提取 token 分类：
- colors
- typography
- rounded
- spacing
- components

### Step 2: 应用混合规则

读取 `data/hybrid-rules.csv`，按规则决定每个 token 类别的处理方式：

| Token 类别 | 策略 | 处理方式 |
|-----------|------|---------|
| colors | selective | 用户指定主色来源；表面色可交叉选取；语义色（success/error）统一 |
| typography | unified | 必须统一使用一个品牌的字体体系，不能混搭 |
| rounded | average | 取两个系统中非 pill 值的中位数或按需选择 |
| spacing | unified | 必须统一使用一个品牌的间距体系 |
| components | pick | 按功能需要从不同品牌中分别选取组件 |
| elevation | unified | 阴影系统必须统一 |
| layout | pick | 布局原则可按需选取 |

### Step 3: 交互式确认

向用户展示混合方案并逐项确认：

```markdown
## 混合方案: Sentry + MiniMax

### Token 来源分配

| Token 类别 | 来源 | 说明 |
|-----------|------|------|
| 色彩 - 主色/强调色 | Sentry | 双极性色彩系统，辨识度高 |
| 色彩 - 表面色 | MiniMax | 白色画布 + 渐变产品卡片 |
| 字体排版 | MiniMax | DM Sans（开源），中文兼容 |
| 圆角 | 折中 | Sentry 18px + MiniMax 32px → 24px |
| 间距 | Sentry | 紧凑间距适合数据密集界面 |
| 组件 | 混合选取 | 卡片/输入框 ← MiniMax，按钮/导航 ← Sentry |
| 阴影/层次 | Sentry | 纹理为主的层次系统 |

### 确认混合方案？
- **A.** 确认，生成混合 DESIGN.md
- **B.** 调整某个 token 的来源
- **C.** 重新选择混合方案
```

### Step 4: 生成混合 Token

根据确认的方案，实际执行 token 混合：

**色彩混合示例：**
```yaml
colors:
  # 来自 Sentry
  primary: "#ff4488"
  on-primary: "#ffffff"
  # 来自 MiniMax
  canvas: "#ffffff"
  canvas-dark: "#0a0a0a"
  # 语义色统一
  success: "#00c853"
  error: "#ff1744"
```

**圆角折中示例：**
```yaml
rounded:
  sm: 8px    # Sentry 6px + MiniMax 8px → 取 MiniMax
  md: 16px   # Sentry 12px + MiniMax 16px → 折中 14px 或取 MiniMax
  lg: 24px   # Sentry 18px + MiniMax 32px → 折中 24px
  pill: 9999px  # 统一
```

**组件选取示例：**
```yaml
components:
  button-primary: {...}    # 来自 Sentry
  card-base: {...}         # 来自 MiniMax
  text-input: {...}        # 来自 MiniMax
  global-nav: {...}        # 来自 Sentry
```

### Step 5: 输出

将混合后的 token 集合传递给 design-generate skill，标注操作类型为「多源混合」，由 design-generate 决定输出路径。

**注意：** 混合生成的 DESIGN.md 输出到用户当前项目根目录（CWD），不是插件目录。

## 混合注意事项

1. **字体绝对不能混** — 如果两个品牌用不同字体，必须选一个。理由：字体是视觉一致性的最大影响因素。
2. **间距绝对不能混** — 间距体系必须统一。理由：间距不一致会破坏视觉节奏。
3. **色彩可以精选** — 不同层次的色彩可以来自不同系统（如主色用 A 的，表面色用 B 的）。
4. **组件可以拼装** — 但需要统一圆角和间距，不能直接复制粘贴。
5. **检查冲突** — 混合后检查是否有 token 引用了不存在的颜色或字体。
