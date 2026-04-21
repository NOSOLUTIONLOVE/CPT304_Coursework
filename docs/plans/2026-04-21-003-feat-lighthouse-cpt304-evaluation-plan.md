---
title: Lighthouse 十项目 CPT304 多维度综合评估文档
type: feat
status: active
date: 2026-04-21
origin: docs/brainstorms/2026-04-21-lighthouse-cpt304-evaluation-requirements.md
---

# Lighthouse 十项目 CPT304 多维度综合评估文档

## Overview

基于 `docs/brainstorms/2026-04-21-lighthouse-cpt304-evaluation-requirements.md` 与现有 `docs/reports/lighthouse-comparative-analysis.md`，新增一份**面向选题与风险管理**的综合评估 Markdown，对齐 CPT304 评分与 5 项基准，并补充环境配置与「原型→专业版」难度判断。

## Problem Frame

仅有 Lighthouse 分数表不足以支撑小组在截止日前做工作量与风险权衡；需将工具数据与课程要求、技术栈文档编织成可读决策参考。

## Requirements Trace

- R1–R2：数据来源与 5 项基准逐项讨论 — 见 **Unit 1**
- R3–R6：评分维度、达标难度、环境、缺陷叙事 — 见 **Unit 1–2**

## Scope Boundaries

- 不运行新的 Lighthouse；不部署到 Vercel/Render。

## Context & Research

### Relevant Code and Patterns

- `docs/reports/lighthouse-comparative-analysis.md`
- `CPT304_Coursework/CPT304_Coursework_01_Details.md`
- `Open_Source_Project/README.md`

### Institutional Learnings

- 无。

## Key Technical Decisions

- **单文件输出**：`docs/reports/lighthouse-cpt304-multi-criteria-evaluation.md`。
- **结构**：摘要 → 作业约束速览 → 评估维度说明 → 十项目逐表/逐段 → 横向对比矩阵 → 风险与选题策略 → 来源与局限 → **审查摘要（ce:review）**。

## Implementation Units

- [x] **Unit 1：撰写主报告（作业对齐 + 维度框架 + 逐项目评估）**

**Goal:** 创建综合评估正文，满足 R1–R6。

**Requirements:** R1–R6

**Dependencies:** 无

**Files:**
- Create: `docs/reports/lighthouse-cpt304-multi-criteria-evaluation.md`

**Approach:**
- 引用 comparative 报告中的分数表（可用摘要复述 + 指向原表）。
- 每项目或每类技术栈给出：Lighthouse 与无障碍 90 的缺口、5 基准相对难度、环境分级、课程叙事友好度。

**Test scenarios:**
- Test expectation: none — 文档交付；人工核对可追溯性。

**Verification:**
- 含 5 项基准对照、10 项目覆盖、局限声明。

- [x] **Unit 2：补充决策矩阵与 ce:review 自检段**

**Goal:** 增加总览矩阵（项目 × 多列风险/难度）；撰写审查摘要（假设性 ce:review）。

**Requirements:** R2, R5, success criteria

**Dependencies:** Unit 1

**Files:**
- Modify: `docs/reports/lighthouse-cpt304-multi-criteria-evaluation.md`

**Verification:**
- 矩阵列与正文无矛盾；审查段列出已检查项与残留假设。

## Sources & References

- **Origin:** [docs/brainstorms/2026-04-21-lighthouse-cpt304-evaluation-requirements.md](docs/brainstorms/2026-04-21-lighthouse-cpt304-evaluation-requirements.md)
- [docs/reports/lighthouse-comparative-analysis.md](../reports/lighthouse-comparative-analysis.md)
