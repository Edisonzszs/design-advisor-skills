<div align="center">

# Design Advisor Skills

**Intelligent design system advisor — 71 brand DESIGN.md files with AI-powered scoring, recommendation, and hybrid generation.**

[![DESIGN.md Count](https://img.shields.io/badge/DESIGN.md%20count-71-10b981?style=classic)](https://github.com/Edisonzszs/design-advisor-skills)
[![Last Update](https://img.shields.io/github/last-commit/Edisonzszs/design-advisor-skills?label=Last%20update&style=classic)](https://github.com/Edisonzszs/design-advisor-skills)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

</div>

---

## What is this?

A cross-platform AI coding agent plugin that helps you **choose the right design system** for your project. Instead of browsing 71 DESIGN.md files manually, describe your project needs and get data-driven recommendations.

Works with: **Claude Code** · **Codex CLI** · **Gemini CLI** · **Cursor** · **GitHub Copilot CLI**

**Three core capabilities:**

1. **Smart Scoring** — 12-dimension weighted evaluation across all 71 brands (color fit, typography, personality, density, dark mode, components, industry, accessibility, feasibility, uniqueness, scalability, cultural fit)
2. **Custom Generation** — Select a brand and auto-adapt it to your project (Chinese fonts, SaaS components, color tweaks)
3. **Hybrid Mixing** — Combine the best tokens from 2-3 brands (e.g., Linear's color system + Binance's component richness)

## What is DESIGN.md?

[DESIGN.md](https://stitch.withgoogle.com/docs/design-md/overview/) is a plain-text design system document introduced by Google Stitch. AI coding agents read it to generate consistent, pixel-perfect UI.

Drop it into your project root, tell your agent *"build me a page that follows DESIGN.md"*, and get UI that actually matches the design system.

## How to Use

### Option 1: Use a DESIGN.md directly (any agent)

1. Browse `design-md/{brand}/DESIGN.md`
2. Copy the one that fits into your project root
3. Tell your AI coding agent to follow it

### Option 2: Use the Design Advisor (recommended)

Install the plugin for your agent, then trigger the advisor:

#### Claude Code

```bash
# Install from marketplace
/plugin install design-advisor

# Or register this marketplace:
/plugin marketplace add Edisonzszs/design-advisor-marketplace
/plugin install design-advisor@design-advisor-marketplace
```

Then run:
```
> /design-advisor
```

#### Codex CLI

```bash
# Install from plugin marketplace
/plugins
# Search for "design-advisor" and select Install
```

Or add to your Codex config:
```bash
# The plugin auto-loads via AGENTS.md when cloned into your project
```

#### Gemini CLI

```bash
# Install the extension
gemini extensions install https://github.com/Edisonzszs/design-advisor-skills
```

Then use `activate_skill design-advisor` or simply ask about design system choices.

#### Cursor

```text
# In Cursor Agent chat:
/add-plugin design-advisor
```

Or search for "design-advisor" in the plugin marketplace.

#### GitHub Copilot CLI

```bash
# Register the marketplace
copilot plugin marketplace add Edisonzszs/design-advisor-marketplace

# Install the plugin
copilot plugin install design-advisor@design-advisor-marketplace
```

### Option 3: Hybrid two brands

Can't decide? Mix the best parts:

```
> /design-advisor

# Select "Hybrid" when prompted
# e.g., Linear's color + Binance's components → new DESIGN.md
```

## Plugin Architecture

```
design-advisor-skills/
├── .claude-plugin/plugin.json       # Claude Code plugin manifest
├── .codex-plugin/plugin.json        # Codex CLI plugin manifest
├── .cursor-plugin/plugin.json       # Cursor plugin manifest
├── gemini-extension.json            # Gemini CLI extension config
├── AGENTS.md                        # Codex entry point
├── GEMINI.md                        # Gemini CLI entry point
├── hooks/
│   ├── hooks.json                   # Claude Code hook config
│   ├── hooks-cursor.json            # Cursor hook config
│   └── session-start                # Multi-platform session init
├── skills/
│   ├── design-advisor/
│   │   ├── SKILL.md                 # Main entry: requirements → scoring → recommendation
│   │   └── references/              # Platform tool mappings
│   │       ├── codex-tools.md
│   │       ├── gemini-tools.md
│   │       └── copilot-tools.md
│   ├── design-score/SKILL.md        # Scoring engine (12 dimensions)
│   ├── design-compare/SKILL.md      # Comparison report + analysis
│   ├── design-hybrid/SKILL.md       # Multi-brand token mixing
│   └── design-generate/SKILL.md     # DESIGN.md output generator
├── data/
│   ├── design-index.yaml            # 71 brands quantitative metrics
│   ├── personality-tags.yaml        # 71 brands personality profiles
│   ├── scoring-dimensions.csv       # 12 scoring dimensions + weights
│   ├── industry-fit.csv             # Industry-style matching matrix
│   └── hybrid-rules.csv             # Token mixing rules
└── design-md/                       # 71 brand DESIGN.md files
```

## Scoring Dimensions

| Dimension | Weight | Category |
|-----------|:------:|----------|
| Color fit | 3 | Visual |
| Typography fit | 3 | Visual |
| Personality match | 3 | Brand |
| Industry fit | 3 | Domain |
| Density suitability | 2 | Layout |
| Dark mode support | 2 | Technical |
| Component coverage | 2 | Technical |
| Feasibility | 2 | Technical |
| Cultural fit | 2 | Domain |
| Accessibility | 1 | Technical |
| Uniqueness | 1 | Brand |
| Scalability | 1 | Technical |

## Brand Collection

### AI & LLM

Claude · Cohere · ElevenLabs · MiniMax · Mistral AI · Ollama · OpenCode AI · Replicate · Runway · Together AI · VoltAgent · xAI

### Developer Tools

Cursor · Expo · Lovable · Raycast · Superhuman · Vercel · Warp

### Backend & Infrastructure

ClickHouse · Composio · HashiCorp · MongoDB · PostHog · Sanity · Sentry · Supabase

### Productivity & SaaS

Cal.com · Intercom · Linear · Mintlify · Notion · Resend · Zapier

### Design & Creative

Airtable · Clay · Figma · Framer · Miro · Webflow

### Fintech

Binance · Coinbase · Kraken · Mastercard · Revolut · Stripe · Wise

### Consumer & Media

Airbnb · Apple · HP · IBM · Meta · Nike · NVIDIA · Pinterest · PlayStation · Shopify · Slack · SpaceX · Spotify · Starbucks · Tesla · The Verge · Uber · Vodafone · WIRED

### Automotive

BMW · BMW M · Bugatti · Ferrari · Lamborghini · Renault

## What's Inside Each DESIGN.md

Every file follows the [Stitch DESIGN.md format](https://stitch.withgoogle.com/docs/design-md/format/):

| Section | What it defines |
|---------|----------------|
| YAML Front Matter | Colors, typography, spacing, rounded, components as tokens |
| Overview | Design philosophy, key characteristics |
| Colors | Brand, surface, text, semantic color roles |
| Typography | Font families, size/weight/spacing hierarchy |
| Layout | Spacing system, grid, whitespace philosophy |
| Elevation | Shadow/surface hierarchy |
| Components | Buttons, cards, inputs, navigation specs |
| Do's and Don'ts | Design guardrails |
| Responsive | Breakpoints, touch targets, collapsing |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). PRs for improving existing DESIGN.md files, adding personality tags, or fixing scoring data are welcome.

## License

MIT — see [LICENSE](LICENSE)
