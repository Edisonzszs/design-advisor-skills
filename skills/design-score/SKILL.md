---
name: design-score
description: "设计评分引擎 - 对 71 个设计系统进行 12 维度评分。由 design-advisor 调用，不直接面向用户。"
---

# Design Score - 评分引擎

本 skill 由 design-advisor 调用，负责对 71 个设计系统进行多维度评分。

## 输入

从调用方接收需求参数：
```yaml
requirements:
  project_type: "AI/LLM产品"
  tone_preference: "科技未来"
  dark_mode: "必须暗色"
  density: "密"
  culture: "中国大陆"
  industry: "AI"
  special_requirements: ""
```

## 评分流程

### 1. 加载数据

读取以下文件：
- `data/design-index.yaml` — 品牌量化指标
- `data/personality-tags.yaml` — 品牌性格标签
- `data/scoring-dimensions.csv` — 维度定义和权重
- `data/industry-fit.csv` — 行业适配矩阵

### 2. 逐品牌评分

对 71 个品牌，按 12 个维度各打 1-10 分：

**color-fit (权重 3)**
- 读取 personality-tags.yaml 中的 color_style
- 读取 industry-fit.csv 中目标行业的 preferred_color_style
- color_style 在 preferred 中 → 8-10 分
- color_style 部分匹配 → 5-7 分
- 不匹配 → 1-4 分
- 额外调整：accent_colors 数量是否符合需求复杂度

**typography-fit (权重 3)**
- 根据 tone_preference 判断适合的字体风格
- 严肃/高端 → hero_size 48-56px, body 15-17px 的品牌高分
- 科技/未来 → hero_size 64+px, 负 letter-spacing 的品牌高分
- 活泼/创意 → 圆角大、色彩丰富的品牌高分
- 检查 font_count 和 proprietary_fonts 判断可行性

**personality-match (权重 3)**
- 读取 personality-tags.yaml 中的 tone 数组
- 与用户的 tone_preference 计算重叠度
- 完全重叠 → 9-10 分
- 部分重叠 → 6-8 分
- 无重叠 → 1-5 分

**density-suitability (权重 2)**
- 读取 personality-tags.yaml 中的 density
- 与用户需求密度匹配
- 完全匹配 → 9-10 分
- 相邻（如"中"vs"密"）→ 6-8 分
- 差两级（如"极疏"vs"密"）→ 1-4 分

**dark-mode (权重 2)**
- 必须暗色：has_dark_mode=true + canvas_polarity=both → 10，=dark → 8，=light → 2
- 需要支持：=both → 10，=dark → 7，=light → 4
- 仅亮色：=light → 10，=both → 7，=dark → 3

**component-coverage (权重 2)**
- 极疏/疏（单页展示）→ component_count >= 15 → 8+，< 15 → 5
- 中（标准页面）→ component_count >= 20 → 8+，< 20 → 5
- 密/极密（后台管理）→ component_count >= 30 → 8+，20-30 → 6，< 20 → 3

**industry-fit (权重 3)**
- 读取 industry-fit.csv，检查品牌的 category 和在 typical_brands 中的出现
- 品牌在目标行业的 typical_brands 中 → 9-10
- 品牌的 category 与目标行业相邻 → 6-8
- 完全不相关 → 1-4

**accessibility (权重 1)**
- canvas_polarity=both 通常有更好的对比度 → 7+
- 高饱和撞色风格 → 4-6
- 单色克制风格 → 7+

**feasibility (权重 2)**
- proprietary_fonts=false → 9-10
- proprietary_fonts=true 但有常见替代（Inter, system-ui）→ 6-8
- proprietary_fonts=true 且独特（SF Pro, Sohne）→ 3-5
- has_gradient=true 增加 SVG 复杂度 → -1

**uniqueness (权重 1)**
- 该行业中视觉辨识度最高的 3-4 个品牌 → 8-10
- 中等辨识度 → 5-7
- 通用风格 → 1-4

**scalability (权重 1)**
- component_count >= 30 且 section_spacing 灵活 → 8-10
- component_count 20-30 → 5-7
- < 20 → 1-4

**cultural-fit (权重 2)**
- culture="中国大陆"：cn_friendly 直接作为主分
- culture="亚太"：cn_friendly × 0.8 + 7 × 0.2
- culture="欧美全球"：反转 cn_friendly 的影响（国际化品牌高分）
- culture="不确定"：全部给 7 分（中性）

### 3. 计算综合分

```
综合分 = Σ(维度分 × 权重) / Σ(权重)
最大权重总和 = 3+3+3+2+2+2+3+1+2+1+1+2 = 25
```

### 4. 输出格式

```json
{
  "scores": [
    {
      "brand": "sentry",
      "composite": 8.72,
      "dimensions": {
        "color-fit": 9,
        "typography-fit": 8,
        "personality-match": 9,
        "density-suitability": 9,
        "dark-mode": 10,
        "component-coverage": 8,
        "industry-fit": 8,
        "accessibility": 7,
        "feasibility": 8,
        "uniqueness": 9,
        "scalability": 8,
        "cultural-fit": 8
      }
    },
    ...
  ],
  "requirements": { ... },
  "timestamp": "2026-05-29T..."
}
```

按综合分降序排列输出。
