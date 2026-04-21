---
title: Lighthouse 多项目对比分析报告整理
type: feat
status: active
date: 2026-04-21
origin: docs/brainstorms/2026-04-21-lighthouse-analysis-report-requirements.md
---

# Lighthouse 多项目对比分析报告整理

## Overview

根据 `docs/brainstorms/2026-04-21-lighthouse-analysis-report-requirements.md`（见 origin），在仓库内新增一份简体中文的 Lighthouse 对比分析报告，数据全部来自 `Lighthouse/` 下各子目录的 JSON（见 `Lighthouse/<项目名>/*.json`），并引用关键未通过审计项说明短板。

## Problem Frame

课程材料已有十次 Lighthouse 运行结果，但缺少一份结构化、可提交的**文字分析**，把分数、指标与项目类型联系起来。

## Requirements Trace

- R1–R2：总览表 + 元数据（版本、时间）— 见 **Unit 1**
- R3：项目类型标注 — 见 **Unit 1** 表格列与 **Unit 2** 叙述
- R4：低分维度与审计项 — 见 **Unit 2**
- R5：方法局限 — 见 **Unit 2**
- R6：简体中文 — 贯穿 **Unit 1–2**

## Scope Boundaries

- 不修改 `Open_Source_Project/` 下应用代码。
- 不新增自动化解析脚本为交付物（规划允许在编写时用一次性命令核对数值）。

## Context & Research

### Relevant Code and Patterns

- 原始运行说明与项目列表：`Open_Source_Project/README.md`
- 数据源：`Lighthouse/*/127.0.0.1_*-*.json` 等（文件名随端口变化）

### Institutional Learnings

- 无既有 `docs/solutions/` 条目。

### External References

- Lighthouse 分类与指标含义以 Google Lighthouse 文档为准（报告中可简述，不依赖外链为必需）。

## Key Technical Decisions

- **单一 Markdown 文件**：`docs/reports/lighthouse-comparative-analysis.md` 作为唯一主交付物（满足 R1 可追溯路径）。
- **百分制展示**：将 JSON 中 `categories.*.score`（0–1）换算为 0–100 整数，与 Lighthouse UI 一致。

## Open Questions

### Resolved During Planning

- 文件路径：定为 `docs/reports/lighthouse-comparative-analysis.md`。

### Deferred to Implementation

- 无。

## Implementation Units

- [x] **Unit 1：撰写报告骨架与数据总览**

**Goal:** 创建 `docs/reports/` 与主报告文件，包含标题、方法说明、Lighthouse 版本与采样时间、全项目对比总表（分类得分 + CWV 摘要 + 项目类型标签）。

**Requirements:** R1, R2, R3, R6

**Dependencies:** 无

**Files:**
- Create: `docs/reports/lighthouse-comparative-analysis.md`

**Approach:**
- 从各 JSON 读取 `lighthouseVersion`、`fetchTime`、`categories.*.score`、`audits` 中 FCP/LCP/TBT/CLS 的 `displayValue`。
- 项目类型标签与 `Open_Source_Project/README.md` 中的表格一致。

**Patterns to follow:**
- 仓库内文档使用 repo-relative 路径指代 `Lighthouse/` 文件。

**Test scenarios:**
- Test expectation: none — 文档撰写无自动化测试；以人工核对「表中数字与 JSON 一致」为完成标准。

**Verification:**
- 表中 10 行项目与 `Lighthouse/` 下 10 个子目录一致；四类得分与 JSON 一致。

- [x] **Unit 2：完成叙述分析、短板归纳与局限说明**

**Goal:** 完成 origin 中的 R4、R5：按维度或项目归纳主要问题，引用代表性 `audits` 标题；撰写方法局限与简短结论。

**Requirements:** R4, R5, R6

**Dependencies:** Unit 1

**Files:**
- Modify: `docs/reports/lighthouse-comparative-analysis.md`

**Approach:**
- 对分类得分低于约 90 或 CWV 明显偏高的项目，从 JSON 提取未通过审计（`score < 1` 且非 notApplicable）的代表项。
- 对比静态页与 Vite/Django 在性能上的差异，避免过度归因。

**Test scenarios:**
- Test expectation: none — 同上。

**Verification:**
- 每个被重点分析的低分维度至少有一条审计标题支撑；局限章节独立成节。

## System-Wide Impact

- 仅新增文档；不影响构建或运行时行为。

## Risks & Dependencies

| Risk | Mitigation |
|------|------------|
| JSON 文件极大，手工抄数易错 | 编写时用脚本或 `jq`/`python` 一次性打印核对 |

## Sources & References

- **Origin document:** [docs/brainstorms/2026-04-21-lighthouse-analysis-report-requirements.md](docs/brainstorms/2026-04-21-lighthouse-analysis-report-requirements.md)
- Related data: `Lighthouse/**/*.json`
