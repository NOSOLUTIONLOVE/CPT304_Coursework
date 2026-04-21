---
title: CPT304 前置工作落地（指南与选型模板）
type: feat
status: active
date: 2026-04-21
origin: docs/brainstorms/2026-04-21-cpt304-prerequisite-work-requirements.md
---

# CPT304 前置工作落地（指南与选型模板）

## Overview

将 `docs/brainstorms/2026-04-21-cpt304-prerequisite-work-requirements.md` 中的「前置工作」落实为可跟随的操作指南与空白模板，便于小组在正式 fork 并进入步骤 2–4 之前统一行动；不修改课程作业 PDF/解析稿原文。

## Problem Frame

作业要求六步流水线，但文档未单独列出「选型前」任务；小组需要一份与评分标准对齐的**前置清单**，减少缺陷类型错误与基准落地失败。

## Requirements Trace

- R1–R2：合规与协作 → 指南中「A. 小组与合规」与「E. 协作与证据」— Unit 1
- R3–R4：概念与评分 → 指南中「B. 必读概念」— Unit 1
- R5–R6：portfolio 与决策 → 指南中「C. 十项目扫描」+ `docs/templates/` 模板 — Unit 2

## Scope Boundaries

- 不 fork 外部仓库、不配置 Vercel；仅新增本仓库内文档。
- 不替小组选定项目或撰写真实文献。

## Context & Research

### Relevant Code and Patterns

- 作业解析：`CPT304_Coursework/CPT304_Coursework_01_Details.md`
- 可选参考：`docs/reports/lighthouse-comparative-analysis.md`（本地 Lighthouse 汇总，非评分要件）

### Institutional Learnings

- 无。

### External References

- 作业所列工具与平台名称以 PDF/解析稿为准（axe、Lighthouse、Codecov、Vercel/Render 等）。

## Key Technical Decisions

- **两份交付物**：① 主指南 `docs/guides/cpt304-prerequisite-work-playbook.md`；② 可复制填写的 `docs/templates/cpt304-project-selection-matrix.md`。
- **阶段划分**：A 合规 → B 概念 → C 十项目扫描 → D 选型决策门 → E fork 与分支约定 → F 与步骤 2 的衔接。

## Open Questions

### Resolved During Planning

- 序列号：同日已有 `2026-04-21-001-*`，本计划为 `002`。

### Deferred to Implementation

- 无。

## Implementation Units

- [x] **Unit 1：撰写前置工作主指南**

**Goal:** 创建 `docs/guides/cpt304-prerequisite-work-playbook.md`，覆盖 R1–R4 及与官方步骤 1–6 的对应关系表。

**Requirements:** R1, R2, R3, R4

**Dependencies:** 无

**Files:**
- Create: `docs/guides/cpt304-prerequisite-work-playbook.md`

**Approach:**
- 用表格列出「前置活动 ↔ 作业步骤 ↔ 报告章节」映射。
- 强调「缺陷 vs 基准」辨析与 AI 使用边界（复述作业要求，不夸大）。

**Patterns to follow:**
- 路径一律相对仓库根目录。

**Test scenarios:**
- Test expectation: none — 纯文档；以人工阅读「是否覆盖 R1–R4」为完成标准。

**Verification:**
- 指南中含合规、概念、时间线、与步骤 2 的衔接五类内容；引用解析稿路径而非臆造分数规则。

- [x] **Unit 2：项目选型矩阵模板 + 完成指南中的 portfolio 节**

**Goal:** 创建 `docs/templates/cpt304-project-selection-matrix.md`，并在主指南中完成 R5–R6（十项目扫描维度、选型决策门、fork 前检查单）。

**Requirements:** R5, R6

**Dependencies:** Unit 1（主指南需链接到模板）

**Files:**
- Create: `docs/templates/cpt304-project-selection-matrix.md`
- Modify: `docs/guides/cpt304-prerequisite-work-playbook.md`

**Approach:**
- 模板含 10 行（对应作业列表项目名 + 空白列：栈、本地运行、预估测试难度、部署风险、备注）。
- 指南中说明如何结合矩阵做「Go/No-Go」与何时执行步骤 1 的 fork。

**Test scenarios:**
- Test expectation: none — 纯文档。

**Verification:**
- 模板可独立复制使用；主指南从模板处可导航。

## System-Wide Impact

- 仅新增/更新 `docs/` 下 Markdown；不影响 `Open_Source_Project/` 代码。

## Risks & Dependencies

| Risk | Mitigation |
|------|------------|
| 与官方 PDF 未来修订不一致 | 文首注明以课程发布的 PDF 为准，解析稿为辅助 |

## Sources & References

- **Origin document:** [docs/brainstorms/2026-04-21-cpt304-prerequisite-work-requirements.md](docs/brainstorms/2026-04-21-cpt304-prerequisite-work-requirements.md)
- [CPT304_Coursework/CPT304_Coursework_01_Details.md](../../CPT304_Coursework/CPT304_Coursework_01_Details.md)
