---
title: Repo 顶层结构重整（导航优先）
type: refactor
status: active
date: 2026-04-28
origin: coursework/docs/brainstorms/2026-04-28-repo-structure-navigation-requirements.md
---

# Repo 顶层结构重整（导航优先）

## Overview

将课程大仓重整为四大顶层区域：`coursework/`、`projects/`、`third_party/`、`artifacts/`，并通过根 `README` + 分区 `README` 索引实现“30 秒定位”。本计划优先保障 **可读性与可回滚性**：先建立新结构与入口，再做迁移与链接重写，最后再考虑移除兼容层。

## Problem Frame

仓库同时承载课程材料、小组可写工作区、十个开源样本副本、以及 Lighthouse 等生成物；目前这些入口分散且语义不够“强”，导致：

- 读者很难快速判断“我该去哪个目录找什么”
- 容易混淆 **样本区** 与 **主开发区**，引发误改/漂移风险
- 文档内部大量硬编码路径，任何重排都可能造成断链

（见 origin：`coursework/docs/brainstorms/2026-04-28-repo-structure-navigation-requirements.md`）

## Requirements Trace

- R1（根入口）— 覆盖于 **U2、U7**
- R2（SSOT 声明）— 覆盖于 **U6、U7**
- R3（目录映射）— 覆盖于 **U3–U6**
- R4（导航索引文件）— 覆盖于 **U2、U7**
- R5（命名与一致性）— 覆盖于 **U1、U2、U7**
- R6（git hygiene）— 覆盖于 **U8**

## Scope Boundaries

- 不引入文档站点生成器（MkDocs/Docusaurus 等）。
- 不在本计划中重构任何应用源码逻辑（仅移动/更新导航与文档链接）。
- 不强制一次性清理所有历史文档措辞；以“高价值入口 + 断链修复”为优先级。

### Deferred to Follow-Up Work

- 大规模内容改写（例如将所有文档示例段落都改成新路径的文字叙述），在迁移完成后视需要进行第二轮清理。

## Context & Research

### Relevant Code and Patterns

- 顶层入口（需同步更新中英双语）：
  - `README.md`
  - `README.zh.md`
- 目录索引 README 的现成样式（可复用为新分区索引）：
  - `inventory-management-system/docs/README.md`
- 样本区与工作区边界的现成阐述（迁移后需更新路径）：
  - `inventory-management-system/README.md`
  - `inventory-management-system/app/WORKSPACE-SOURCE.md`
- Lighthouse 产物与报告的强耦合（路径断链风险最高）：
  - `Lighthouse/`
  - `docs/reports/lighthouse-comparative-analysis.md`
  - `docs/reports/lighthouse-cpt304-multi-criteria-evaluation.md`
- 十项目样本运行总入口（迁移后路径需更新）：
  - `Open_Source_Project/README.md`

### Institutional Learnings

- 未发现 `docs/solutions/` 类知识库；但现有 brainstorm + plan 已明确要求“入口清晰、SSOT 声明、索引 README、避免 `.DS_Store` 噪音”。

### External References

- 无（此为仓库信息架构重排，优先遵循本仓库已有文档风格与课程提交约束）。

## Key Technical Decisions

- **迁移策略**：采用“两阶段迁移 + 兼容层”的低风险路线：
  - 阶段 A：先建立新结构与入口（不影响读者继续使用旧路径）
  - 阶段 B：搬迁目录 + 链接重写 + 清理噪音
- **兼容层形式**：优先使用“旧目录保留一个 `README.md`/`INDEX.md` 指向新位置”的方式，而不是 symlink（ZIP 提交与 Windows 环境更稳）。
- **taxonomy 继承**：保留 `docs/{brainstorms,plans,reports,guides,templates}` 这一已稳定的分类，但将其作为 `coursework/docs/...` 的内部结构延续。

## Open Questions

### Resolved During Planning

- **是否保留旧目录兼容入口？** 是。至少保留一个过渡期：旧目录仅保留“Moved to …”索引文件，避免断链造成协作成本上升。

### Deferred to Implementation

- 过渡期多长（删除旧目录 stub 的时间点）取决于迁移后断链情况与课程提交时间点。

## Output Structure

> *这是目标结构示意，用于对齐“最终长什么样”；迁移过程中允许短期同时存在旧目录与新目录。*

    coursework/
      README.md
      docs/
        brainstorms/
        plans/
        reports/
        guides/
        templates/
      CPT304_Coursework/   # 若该目录确为课程流程/材料，可迁入此处
    projects/
      README.md
      inventory-management-system/
        README.md
        app/
        docs/
    third_party/
      README.md
      open_source_projects/
        README.md
        advanced-finance-tracker/
        ...
    artifacts/
      README.md
      lighthouse/
        inventory-management-system/
        ...

## Implementation Units

- [ ] U0. **明确迁移边界：`CPT304_Coursework/` 是否纳入 `coursework/`**

**Goal:** 决定是否将 `CPT304_Coursework/` 视为课程材料并迁入 `coursework/`，并将该决定写入根入口与迁移策略，避免“搬一半断一半”。

**Requirements:** R1, R3

**Dependencies:** None

**Files:**
- Modify: `README.md`
- Modify: `README.zh.md`
- (Optional) Move: `CPT304_Coursework/` → `coursework/CPT304_Coursework/`
- (Optional compat stub): `CPT304_Coursework/README.md`（Moved to 新位置）

**Approach:**
- 若 `CPT304_Coursework/` 主要是课程流程/材料：纳入 `coursework/`。
- 若其与课程材料无关或迁移成本过高：先保持原地，但根入口必须把它作为“课程材料入口之一”明确链接，避免被遗漏。

**Test scenarios:**
- Test expectation: none — 目录决策与文档导航变更。

**Verification:**
- 根入口能明确找到课程流程文档所在位置；不存在“新结构说搬了但实际没搬/链接没改”的混乱状态。

---

- [ ] U1. **冻结命名与边界规则（避免“搬完又散”）**

**Goal:** 在开始 `git mv` 前，把目录语义与命名规则写死，避免迁移中出现两套标准。

**Requirements:** R5

**Dependencies:** None

**Files:**
- Modify: `coursework/docs/brainstorms/2026-04-28-repo-structure-navigation-requirements.md`（如需补充“命名规则/边界规则”到更明确的一句话）
- Create: `coursework/README.md`（迁移后创建；此 unit 只定义其必须包含哪些段落）

**Approach:**
- 明确“可写区仅 `projects/`，其余默认只读/可再生”。
- 明确 “inventory-management-system” 名称只保留一份主位置（迁移后在 `projects/`）。

**Test scenarios:**
- Test expectation: none — 文档与信息架构约束变更，无运行时行为。

**Verification:**
- 组内任何人能一致回答：什么可以改、什么不要改、每类内容去哪里找。

---

- [ ] U2. **建立新四分区目录与索引 README（先立路标）**

**Goal:** 创建 `coursework/`、`projects/`、`third_party/`、`artifacts/` 与各自 `README.md` 索引，让新导航先可用。

**Requirements:** R1, R4

**Dependencies:** U1

**Files:**
- Create: `coursework/README.md`
- Create: `projects/README.md`
- Create: `third_party/README.md`
- Create: `artifacts/README.md`

**Approach:**
- 复用 `inventory-management-system/docs/README.md` 的“表格索引 + 简短说明”风格。
- 每个索引 README 至少包含：用途一句话、关键入口链接、更新规则（尤其 `artifacts/` 的可再生说明）。

**Test scenarios:**
- **Happy path:** 从根目录进入任一分区 README，能在 2 次点击内到达该分区的关键内容。

**Verification:**
- 4 个分区目录存在且 README 互相链接不产生循环迷路（每个分区都能回到根入口或主索引）。

---

- [ ] U3. **迁移课程文档：`docs/` → `coursework/docs/`（保留 taxonomy）**

**Goal:** 将课程类文档统一迁入 `coursework/`，并保留原有 `docs/*` 分类结构。

**Requirements:** R3

**Dependencies:** U2

**Files:**
- Move: `docs/` → `coursework/docs/`
- Modify (as needed): `coursework/docs/**.md`（修复内部相对链接）
- Create (compat stub): `docs/README.md`（Moved to `coursework/docs/` 的入口与常用映射）
  - Include: `docs/INDEX.md`（可选：列出常见旧路径 → 新路径）

**Approach:**
- 先整体搬迁，再做链接修复（避免边搬边改造成遗漏）。
- 对“高价值入口”优先修复：`coursework/docs/README`（若新增）、`coursework/docs/reports/*`、`coursework/docs/guides/*`。
- **必须处理 plans frontmatter 与深链：** `docs/plans/*.md` 在迁移后会变成 `coursework/docs/plans/*.md`，其 frontmatter 的 `origin:` 以及正文内的 `docs/...` 链接，需要统一重写为新路径前缀（例如 `coursework/docs/brainstorms/...`）。

**Test scenarios:**
- **Happy path:** 从 `coursework/docs/brainstorms/` 与 `coursework/docs/plans/` 互相跳转不出现 404。
- **Edge case:** 旧 `docs/README.md` 仍可把读者导向新位置，不要求深链接全部可用（过渡期允许）。

**Verification:**
- `docs/` 旧入口存在且明确“已迁移”；`coursework/docs/` 成为唯一权威路径。
- `coursework/docs/plans/*.md` 的 `origin:` 不再指向旧的 `docs/brainstorms/...`。

---

- [ ] U4. **迁移生成物：`Lighthouse/` → `artifacts/lighthouse/`**

**Goal:** 将审计产物集中到 `artifacts/`，并修复报告对路径的引用。

**Requirements:** R3, R1

**Dependencies:** U2

**Files:**
- Move: `Lighthouse/` → `artifacts/lighthouse/`
- Modify: `coursework/docs/reports/lighthouse-comparative-analysis.md`
- Modify: `coursework/docs/reports/lighthouse-cpt304-multi-criteria-evaluation.md`
- Create (compat stub): `Lighthouse/README.md`（Moved to `artifacts/lighthouse/`）

**Approach:**
- 先搬迁产物目录，再集中修复“报告中硬编码的路径字符串”（这类断链影响最大）。
- `artifacts/README.md` 写清“可再生、不要手改 JSON/HTML 内容”的规则。

**Test scenarios:**
- **Happy path:** 报告中引用的 Lighthouse 文件路径全部指向 `artifacts/lighthouse/...`。

**Verification:**
- Lighthouse 报告 markdown 在 GitHub 预览中不再出现指向旧目录的链接/路径示例。

---

- [ ] U5. **迁移样本区：`Open_Source_Project/` → `third_party/open_source_projects/`**

**Goal:** 将 10 个开源样本明确归类为只读第三方内容，并保持其“运行说明入口”可用。

**Requirements:** R3, R1

**Dependencies:** U2

**Files:**
- Move: `Open_Source_Project/` → `third_party/open_source_projects/`
- Modify: `third_party/open_source_projects/README.md`（确保路径示例与 `cd` 命令更新）
- Create (compat stub): `Open_Source_Project/README.md`（Moved to 新位置 + 常用项目映射）

**Approach:**
- 保留原 `README.md` 内容风格，但将 `<REPO>` 示例改成新路径前缀。
- 在 `third_party/README.md` 明确“只读对照/参考，不作为主开发”的约束。

**Test scenarios:**
- **Happy path:** `third_party/open_source_projects/README.md` 中所有 `cd <REPO>/...` 示例都可直接对应真实路径。

**Verification:**
- 新成员仍可只读一份样本运行说明完成本地启动（无需知道旧目录名）。

---

- [ ] U6. **迁移小组项目：`inventory-management-system/` → `projects/inventory-management-system/`**

**Goal:** 将小组可写工作区放入 `projects/`，同时确保其内部关于“样本 vs 权威”的阐述仍成立并更新路径。

**Requirements:** R2, R3

**Dependencies:** U2, U5

**Files:**
- Move: `inventory-management-system/` → `projects/inventory-management-system/`
- Modify: `projects/inventory-management-system/README.md`
- Modify: `projects/inventory-management-system/app/WORKSPACE-SOURCE.md`
- Modify: `projects/inventory-management-system/docs/README.md`
- Create (compat stub): `inventory-management-system/README.md`（Moved to 新位置）

**Approach:**
- 更新所有提到 `Open_Source_Project/inventory-management-system` 的路径到 `third_party/open_source_projects/inventory-management-system`。
- 在项目 README 中更明确写出 SSOT（例如 fork vs 本仓镜像）与同步策略，以符合 R2。

**Test scenarios:**
- **Happy path:** 读 `projects/inventory-management-system/README.md` 能明确知道：主开发在哪、样本在哪、如何对照。

**Verification:**
- `projects/inventory-management-system/docs/README.md` 的索引链接全部可点通。

---

- [ ] U7. **更新根导航与跨分区入口（中英双语同步）**

**Goal:** 让根目录成为“你要找什么？”的一键入口，并保持中英一致。

**Requirements:** R1, R4, R5

**Dependencies:** U2–U6

**Files:**
- Modify: `README.md`
- Modify: `README.zh.md`

**Approach:**
- 根 README 以四分区为主视角重写“Project Overview / Usage”段落。
- 明确标注：`third_party/` 为只读样本、`projects/` 为可写开发区、`artifacts/` 为生成物、`coursework/` 为课程材料。

**Test scenarios:**
- **Happy path:** 从根 README 出发，2 次点击内到达：
  - `coursework/docs/plans/`
  - `projects/inventory-management-system/`
  - `third_party/open_source_projects/README.md`
  - `artifacts/lighthouse/`

**Verification:**
- 中英文 README 的目录入口一致，不出现一个指向旧路径、另一个指向新路径的情况。

---

- [ ] U8. **断链修复与 hygiene 收尾（含 `.DS_Store`）**

**Goal:** 修复迁移造成的主要断链，并减少结构重排带来的噪音文件与无意义 diff。

**Requirements:** R6

**Dependencies:** U3–U7

**Files:**
- Modify: `.gitignore`（加入 `.DS_Store` 等）
- Delete (tracked): `.DS_Store`、`docs/.DS_Store`（以及迁移中发现的其他同类文件）
- Modify: `coursework/docs/**.md`、`projects/**.md`、`third_party/**.md`（按需批量修复残留旧路径字符串）
  - Decide: `.cursor/settings.json` 是否应纳入版本控制（若不应纳入，则加入 `.gitignore` 并保持本地；若应纳入，则在根 README 或 `coursework/README.md` 中写明其用途与团队约定）

**Approach:**
- 以“入口文件 + 报告 + 运行说明”为优先级修复路径引用。
- 将旧目录 stub 保留到稳定后再移除，避免 PR 中断链修复量过大。
- **stub 兼容层硬标准：** 旧目录（`docs/`、`Lighthouse/`、`Open_Source_Project/`、`inventory-management-system/` 等）在过渡期只允许保留“Moved to …”索引文件，不再承载真实内容，避免双路径漂移。

**Test scenarios:**
- Test expectation: none — 主要为路径与仓库卫生；以“链接可达”作为验收。

**Verification:**
- 仓库内不再追踪 `.DS_Store`；主要入口文档不再引用旧路径（或旧路径只出现在 stub 的“Moved to …”里）。
- 旧目录中不存在与新目录重复的“真实内容”（仅保留 stub/index）。

## System-Wide Impact

- **Interaction graph:** 影响所有“以路径作为信息架构”的入口：根 README、`Open_Source_Project/README.md`、Lighthouse 报告、inventory 工作区文档索引。
- **Unchanged invariants:** 各子项目源码本身不改逻辑；仅移动位置与更新文档引用。
- **ZIP/提交风险:** symlink 不作为依赖；兼容层以 markdown stub 实现，保证跨平台解压可读。

## Risks & Dependencies

| Risk | Mitigation |
|------|------------|
| 断链太多导致迁移不可用 | 分阶段迁移 + stub 兼容层；优先修复高价值入口 |
| 中英 README 不一致 | U7 强制同步更新；验收里把“两份 README 同路径入口”列为硬标准 |
| Lighthouse 报告路径大量硬编码 | U4 明确将报告文件列为必改文件，并在验收中要求无旧路径残留 |
| `.DS_Store` 噪音在迁移中扩散 | U8 将 `.gitignore` 与删除 tracked 文件作为收尾 gate |

## Documentation / Operational Notes

- 迁移是“信息架构变更”，建议以一次集中 PR 完成（或拆成 2 个 PR：先 U2/U7，再 U3–U8），以减少长期双路径共存的困惑。

## Sources & References

- **Origin document:** [coursework/docs/brainstorms/2026-04-28-repo-structure-navigation-requirements.md](coursework/docs/brainstorms/2026-04-28-repo-structure-navigation-requirements.md)
- Existing index pattern: `inventory-management-system/docs/README.md`
