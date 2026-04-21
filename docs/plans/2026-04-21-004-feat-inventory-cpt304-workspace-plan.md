---
title: Inventory Management System CPT304 工作区、环境与「专业版」推进计划
type: feat
status: completed
date: 2026-04-21
origin: docs/brainstorms/2026-04-21-inventory-management-system-workspace-requirements.md
---

# Inventory Management System CPT304 工作区、环境与「专业版」推进计划

## Overview

在课程仓库中落地 **`inventory-management-system/` 工作区**，串联 **开发环境安装**、**源代码缺陷识别（步骤 2）**、**缺陷导向文献（步骤 3）** 与 **「专业版」重构与五项基准（步骤 4）** 的分工与证据占位。应用本体仍以 **Inventory Management System (UNIQLO)**（官方列表 `sptin2002/inventory-management-system`）为 fork 目标；本计划不强制重写业务功能，聚焦协作载体与流程。

## Problem Frame

小组已选定课题，但需要可追溯的本地/仓库内结构，避免缺陷、文献与重构基准在组内口头传递中丢失；同时需对齐 CPT304 对「真缺陷」与「步骤 4 基准」的区分（见 `docs/brainstorms/2026-04-21-cpt304-prerequisite-work-requirements.md`）。

## Requirements Trace

- R1–R2：工作区根与源码权威策略 — 见 **Unit 1**
- R3–R4：模块化责任域的可落盘载体 — 见 **Unit 1、6**
- R5–R6：步骤 2–4 的文档化流程 — 见 **Unit 2–5**

## Scope Boundaries

- 不在本计划阶段完成四座缺陷的**代码修复**或四篇文献的**定稿全文**；只建立模板、清单与 runbook。
- 不运行新的 Lighthouse 或强制部署；部署与覆盖率数字在后续执行迭代中填入证据链。

### Deferred to Separate Tasks

- **小组 fork 创建与 Git 协作细则：** 在 GitHub 上完成 fork、branch 策略；本仓库仅 **文档化** 推荐做法。

## Context & Research

### Relevant Code and Patterns

- `Open_Source_Project/inventory-management-system/` — 课程 bundled 样本（单页、jQuery、DataTables、`DB.txt` 持久化）。
- `Open_Source_Project/README.md` §7 — 本地运行方式。
- `docs/reports/lighthouse-comparative-analysis.md`、`docs/reports/lighthouse-cpt304-multi-criteria-evaluation.md` — 已知无障碍与性能热点（如 `html` 缺 `lang`、缺 main landmark、Legacy JS 等）；**审计缺陷须基于源码独立论证**，不能仅复述 Lighthouse 条目作为四缺陷的唯一依据。
- `docs/guides/cpt304-prerequisite-work-playbook.md` — 前置流程对齐。

### Institutional Learnings

- 无 `docs/solutions/` 条目。

### External References

- 课程 PDF / `cpt304-coursework-01-complete-workflow.md` 中的步骤与五项基准定义（规划时以课程为准）。

## Key Technical Decisions

- **源码权威：** 推荐以 **小组 fork** 为日常开发与 PR 的主仓库；`inventory-management-system/app/README.md` 说明是否在本仓库镜像子树或仅保留链接，避免无文档的双主库（见 origin R1–R2）。
- **模块化：** 与 brainstorm 一致，按 **壳与无障碍 / 表格与 CRUD / 持久化 / 样式 / 质量与基准** 分工；工作区用 `docs/module-owners.md` 固化认领。
- **测试与覆盖率：** 纯静态 + 全局脚本对「行覆盖率」不友好；步骤 4 达标需引入 **可统计覆盖** 的运行方式（例如打包后 ISTANBUL + 单测，或 E2E + 覆盖率工具）。具体选型在 **Unit 5** 与实现阶段结合 fork 实况决定，本计划只要求 **提前在 checklist 中记录选型结论位**。

## Open Questions

### Resolved During Planning

- **工作区是否包含应用源码副本？** 以 **可选镜像** 处理：默认 fork 为权威；若课程提交要求单 zip 内含完整源码，再在 `app/` 同步 fork 的快照并在 README 标注日期与 commit。
- **四项缺陷从哪里找？** 从 **行为与源码** 找：业务逻辑、边界数据、可访问性失败、性能/安全反模式（在样本中需谨慎区分「基准缺失」与「真缺陷」）。

### Deferred to Implementation

- 最终部署平台（Vercel/Netlify/GitHub Pages 等）与构建命令。
- 双语言具体内容与翻译范围（全站 UI 字符串表 vs. 最小对照页）。

## Implementation Units

- [x] **Unit 1: 工作区目录与权威说明**

**Goal:** 创建 `inventory-management-system/` 根结构，说清与 `Open_Source_Project/inventory-management-system` 及 fork 的关系。

**Requirements:** R1, R2, R5

**Dependencies:** None

**Files:**
- Create: `inventory-management-system/README.md`
- Create: `inventory-management-system/app/README.md`
- Modify: none

**Approach:**
- 根 README：官方列表 URL、课程矩阵中的名称、工作区子目录用途、推荐 Git 协作句段。
- `app/README.md`：声明源码主来源（fork）、何时向本目录同步、相对路径链到课程样本以便对照。

**Patterns to follow:**
- `Open_Source_Project/README.md` 的写法粒度。

**Test scenarios:**
- **Happy path:** 新成员只读两份 README 能回答「到哪里 clone」「本目录 app 是否可编辑」。
- **Edge case:** 若组内暂时未 fork，`app/` 说明指向课程样本用于本地练习时的限制。

**Verification:**
- 目录存在且 README 无 broken 的 repo-relative 路径引用。

---

- [x] **Unit 2: 开发环境安装 Runbook**

**Goal:** 写清在不同 OS 上运行 **bundled 样本** 与 **fork 检出** 的最低步骤。

**Requirements:** R5

**Dependencies:** Unit 1

**Files:**
- Create: `inventory-management-system/docs/dev-environment.md`

**Approach:**
- 静态站：Python `http.server`、VS Code Live Server、`npx serve` 等选一两种；说明 CORS/本地 `file://` 不适用的情况。
- 记录推荐浏览器与 DevTools（Accessibility 面板用于后续 a11y 基准）。
- Node：标注「仅在选择引入构建/测试工具时需要」，版本范围从实现阶段确认。

**Test scenarios:**
- **Happy path：** 按文档从 0 到可在浏览器打开 `index.html` 等价物。
- **Error path：** `file://` 打开失败或不完全时，指向 HTTP 服务方案。

**Verification:**
- 组外一人可照文档完成打开页面（无需额外口头说明）。

---

- [x] **Unit 3: 缺陷识别方法与登记模板**

**Goal:** 支撑步骤 2：识别 **四座源代码内独立缺陷**，并与「基准缺失」区分。

**Requirements:** R5, R6

**Dependencies:** Unit 1

**Files:**
- Create: `inventory-management-system/docs/defect-identification.md`
- Create: `inventory-management-system/docs/templates/defect-log-template.md`

**Approach:**
- 明文列出 **不合格** 的缺陷例子（如仅「未做 i18n」）与 **合格** 例子（如某输入未校验导致错误导出）。
- 提供四行登记模板：现象、复现、根因（源码位置域级描述）、计划修复、对应文献编号占位。
- 引用现有 Lighthouse 报告仅作 **线索**，每座缺陷须在源码或运行行为中可指认。

**Test scenarios:**
- **Happy path：** 四座行均可填写且无与 R6 冲突的占位。
- **Edge case：** 两个候选缺陷实为同一根因时，合并规则说明。

**Verification:**
- 模板可直接复制到作业报告初稿或组内 wiki。

---

- [x] **Unit 4: 文献查找流程与矩阵模板**

**Goal:** 支撑步骤 3：每缺陷对应 **学术或行业可信来源**，可追溯 APA/HKU 格式占位。

**Requirements:** R5

**Dependencies:** Unit 3

**Files:**
- Create: `inventory-management-system/docs/literature-workflow.md`
- Create: `inventory-management-system/docs/templates/literature-matrix-template.md`

**Approach:**
- 检索策略：关键词由「缺陷类型 + 上下文」（如 accessibility table、client-side data validation）组合；优先近五年与可引用 DOI/ISBN。
- 矩阵列：**缺陷 ID、文献编号、如何支撑修复决策、冲突处理**。
- 强调 **不抄袭**：课程 AI 与学术诚信约束在文内短提醒 + 链到课程原文位置（若已在别册则只写索引句）。

**Test scenarios:**
- **Happy path：** 四条缺陷行各对应至少一条文献占位。
- **Integration：** 文献论点与缺陷修复在报告中的一致性检查清单一句。

**Verification:**
- 组长可用矩阵跟踪「四缺陷 × 四文献」完成度。

---

- [x] **Unit 5: 「专业版」与五项基准 Checklist**

**Goal:** 将步骤 4 五项基准映射到 **inventory-management-system** 的可执行检查项与证据占位。

**Requirements:** R3, R5

**Dependencies:** Unit 1–2

**Files:**
- Create: `inventory-management-system/docs/professional-edition-checklist.md`

**Approach:**
- 逐项：在线 ≥7 天、测试覆盖率 ≥80%、Lighthouse 无障碍 ≥90、双语言、Cookie + 隐私页 — 每条包含 **负责人占位、证据 URL/截图路径占位、工具占位**。
- 写入本项目已知难点（legacy JS、a11y 分差）与 **缓解方向**（语义 landmark、减少 render-blocking、引入测试管线）但不预写具体补丁。

**Test scenarios:**
- **Happy path：** 五项均有「未完成/进行中/完成」勾选位。
- **Edge case：** 静态托管下 Cookie 横幅的最小合规实现选项一句（不含法律建议，仅工程选项）。

**Verification:**
- checklist 与 brainstorm 中 R3 责任域可对号入座。

---

- [x] **Unit 6: 模块化认领表**

**Goal:** 将 brainstorm 中的功能域分工固化为可编辑表。

**Requirements:** R3, R4

**Dependencies:** Unit 1

**Files:**
- Create: `inventory-management-system/docs/module-owners.md`

**Approach:**
- 行：壳与无障碍、表格与 CRUD、本地持久化、样式与品牌、质量与五项基准；列：主负责人、备份、主要对接文件类型（HTML/CSS/JS）、备注。

**Test scenarios:**
- **Happy path：** 四名成员可各领一行且覆盖五项基准对接列不空。

**Verification:**
- `module-owners.md` 与 `professional-edition-checklist.md` 责任人字段可互相引用（姓名或学号策略由组内定）。

## System-Wide Impact

- **Interaction graph：** 仅新增文档与 `inventory-management-system/`；不修改 `Open_Source_Project/inventory-management-system` 行为。
- **Unchanged invariants：** 课程十项目打包路径与现有 Lighthouse JSON 保持不变。

## Risks & Dependencies

| Risk | Mitigation |
|------|------------|
| 双主库（fork 与本仓库 `app/`）分歧 | Unit 1 README 强制写同步策略与频率 |
| 覆盖率工具链过晚导致返工 | checklist 中提前锁定「是否引入构建」决策截止日 |
| 缺陷选成基准缺失 | Unit 3 文案与课程 4.3 辨析对齐 |

## Documentation / Operational Notes

所有新文件均位于 `inventory-management-system/docs/`，便于最终与 `GroupXX.zip` 一并收集时单独打包。

## Sources & References

- **Origin document:** [docs/brainstorms/2026-04-21-inventory-management-system-workspace-requirements.md](docs/brainstorms/2026-04-21-inventory-management-system-workspace-requirements.md)
- Related: [docs/brainstorms/2026-04-21-cpt304-prerequisite-work-requirements.md](docs/brainstorms/2026-04-21-cpt304-prerequisite-work-requirements.md)
- Related code sample: `Open_Source_Project/inventory-management-system/`
