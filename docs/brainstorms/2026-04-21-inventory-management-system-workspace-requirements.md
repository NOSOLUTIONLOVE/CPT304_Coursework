---
date: 2026-04-21
topic: inventory-management-system-workspace
---

# CPT304：inventory-management-system 小组工作区与模块化协作

**课程依据：** `CPT304_Coursework/cpt304-coursework-01-complete-workflow.md` 与作业稿 six-step 流程（选型 → 审计 → 文献 → 重构 → 报告 → 提交）。

**选定上游（官方 2.6.1 列表）：** Inventory Management System (UNIQLO) — `https://github.com/sptin2002/inventory-management-system`（与课程矩阵一致）。

## Problem Frame

小组已锁定 **inventory-management-system**。当前课程仓库内已有 **`Open_Source_Project/inventory-management-system`** 作为十项目 bundled 副本，但 **缺少面向小组 fork 与分工的独立工作区**：文档、缺陷追踪、文献与「专业版」迭代若散落在即时通讯中，容易与**源代码缺陷**和**步骤 4 五项基准**脱节。需要在仓库根目录建立专用文件夹，用 **模块化责任域** 支撑并行开发，并与课程步骤对齐。

## Requirements

**仓库布局与单一事实来源**

- R1. 在仓库根目录维护目录 `inventory-management-system/`，作为本组课题的**工作区根**：须说明与 `Open_Source_Project/inventory-management-system`（课程只读样本）及 **小组 GitHub fork** 的关系，避免两套代码长期漂移且无说明。
- R2. 工作区内至少包含：**根级说明**、**课程/协作用文档区**，以及 **应用源码占位策略**（源码以 fork 为主开发，或在本仓库 `app/` 下跟踪；由计划在实现阶段二选一或组合并写清同步方式）。

**模块化协作（语义上的模块，而非强制技术栈）**

- R3. 按功能域划分组内责任，与现有单页 + jQuery + DataTables 结构一致，便于并行：
  - **壳与语义**：文档结构、landmark、`lang`、标题层级（支撑 Lighthouse 无障碍基准）。
  - **数据表与 CRUD**：DataTables 配置、增删改与导出逻辑。
  - **本地持久化**：`DB.txt` / `LOAD DATA` / `SAVE DATA` 行为与边界情况。
  - **样式与品牌**：Bootstrap / 自定义 CSS、响应式与对比度。
  - **质量与交付**：测试脚手架、覆盖率、部署与双语言、Cookie/隐私页（步骤 4 五项基准的落地分工）。
- R4. 模块边界以 **可交付物与接口约定** 为主（例如：持久化层输入输出格式、表格列约定），具体文件拆分在规划阶段随重构策略确定，不在此冻结目录树细节。

**与课程步骤对齐**

- R5. 工作区须能支撑：**步骤 2** 四座「真缺陷」（源代码内独立问题，非步骤 4 才要求的基准缺失）、**步骤 3** 与缺陷一一对应的文献线索、**步骤 4** 「专业版」重构与五项最低基准的证据存放位置。
- R6. 不把 i18n、Cookie 横幅、隐私页等 **步骤 4 才要求的基准** 误报为步骤 2 的审计缺陷（与前置需求 R3 一致）。

## Success Criteria

- 组员能指着 `inventory-management-system/` 说明：**在哪写缺陷描述**、**在哪记文献**、**正式改代码的主仓库是 fork 还是本目录 `app/`**。
- 至少完成 **四个功能责任域** 的认领表或等价说明，且与「专业版」五项基准中的高风险项（对本项目：无障碍、覆盖率、静态站测试策略等）有可追溯分工。

## Scope Boundaries

- 不代替课程要求的 **正式审计报告 PDF**、**四篇定稿文献**或 **最终部署 URL**；本工作区提供**占位与流程载体**。
- 不在需求阶段强制引入新框架（如整站改写为 React）；若组内后续选择较大技术迁移，应单独再开一轮选型/计划。

## Key Decisions

- **课题官方名称与列表行**：采用 **Inventory Management System (UNIQLO)** / `inventory-management-system` 行，与 `docs/templates/cpt304-project-selection-matrix.md` 第 3 行一致。
- **模块化优先**：优先按 **功能域 + 里程碑** 分工，而非按「每人改几行 index.html」；减少合并冲突与基准遗漏。
- **工作区与样本副本关系**：`Open_Source_Project/inventory-management-system` 保留为课程打包参照；小组日常开发以 **fork** 或 **本仓库 app 目录**之一为权威，并在 `inventory-management-system/README.md` 中写死，避免双主库。

## Dependencies / Assumptions

- 假设小组将对官方列表仓库进行 **fork** 并取得写权限。
- 假设全员能使用静态 HTTP 服务（如 VS Code Live Server）运行当前纯前端应用。

## Outstanding Questions

### Resolve Before Planning

- （无阻塞项；源码主目录二选一在规划中明确即可。）

### Deferred to Planning

- **[Needs research]** 测试覆盖率 ≥80% 对纯静态+jQuery 项目的可行工具链（如是否引入构建与单测框架）。
- **[Technical]** 双语言与 Cookie/隐私页在静态托管上的最小实现形态（同构多 HTML vs. 轻量 i18n 字典）。

## Next Steps

-> `/ce:plan`：将开发环境、缺陷识别、文献流程与「专业版」开发拆解为可执行计划与工作区文件清单。
