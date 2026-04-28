# Inventory Management System (UNIQLO) — 小组工作区

本目录是 CPT304 课题 **Inventory Management System (UNIQLO)** 在课程仓库内的**协作与文档根**，与课程打包的只读样本区分开，便于管理分工、缺陷、文献与「专业版」证据链。

## 官方课程列表与上游

- **列表中的名称：** Inventory Management System (UNIQLO)  
- **官方列表仓库（fork 源）：** https://github.com/sptin2002/inventory-management-system  
- **课程选型矩阵模板：** `coursework/docs/templates/cpt304-project-selection-matrix.md`（第 3 行）

在矩阵「最终决议」与下方写清：**选题上游** 与 **本组实际提交/协作的 Git 远程** 可能不是同一个仓（本组若整门课只维护一个大仓库，见下表「小组主仓库」）。

| 项目 | 内容 |
|------|------|
| 小组主仓库（本作业协作用） | <https://github.com/NOSOLUTIONLOVE/CPT304_Coursework> |
| 说明 | 上列为 CPT304 课程大仓（含 `third_party/open_source_projects/`、`projects/inventory-management-system/app/` 等），**不是**在 GitHub 上对 `sptin2002/inventory-management-system` 的「Fork」按钮产出的子仓库；若老师要求另 fork 上游应用再开发，可再增加一行并保留本仓为作业打包提交地址。 |
| 默认开发分支 | 建议 `main` / `develop`，组内统一即可 |

## 与 `third_party/open_source_projects/inventory-management-system` 的关系

`third_party/open_source_projects/inventory-management-system/` 是嵌入本仓库的**课程样本**，用于横向对比与本地快速打开，**一般不作为长期主开发分支**，除非你组明确选择以课程仓库为唯一提交物。

推荐做法：

1. **主开发在小组主仓库**（如 <https://github.com/NOSOLUTIONLOVE/CPT304_Coursework>）：日常 PR、Issue 与本课题的 `projects/inventory-management-system/` 修改以该远程为准。  
2. **本目录 `app/`**：说明与 fork 的同步方式（快照 / 子模块 / 仅链接）；见 `app/README.md`。  
3. 需要对照原版行为时，再打开 `third_party/` 中的副本。

## 目录结构

| 路径 | 用途 |
|------|------|
| `app/` | **完整应用源码**（自 `third_party/open_source_projects/inventory-management-system/` 迁入）；入口 `index.html`、`assets/`、`DB.txt`、`LICENSE` 等 |
| `app/WORKSPACE-SOURCE.md` | 快照来源、fork 关系、同步记录 |
| `app/README.md` | 上游项目说明 + 顶栏指向本工作区策略 |
| `docs/dev-environment.md` | 开发环境与本地运行 |
| `docs/defect-identification.md` | 步骤 2：四座「真缺陷」识别方法 |
| `docs/templates/defect-log-template.md` | 缺陷登记模板 |
| `docs/literature-workflow.md` | 步骤 3：文献检索与引用流程 |
| `docs/templates/literature-matrix-template.md` | 缺陷–文献对应矩阵 |
| `docs/professional-edition-checklist.md` | 步骤 4：五项最低基准与证据占位 |
| `docs/module-owners.md` | 模块化功能域与负责人 |
| `docs/code-review-inventory-app-2026-04-27.md` | `app/` 人工源码审查记录（步骤 2 候选依据，正式报告前须复现核实） |

## 模块化协作（概要）

按 **功能域** 并行，而非「每人随便改 `index.html`」，减少合并冲突。五域说明见 `docs/module-owners.md`，对应作业中的无障碍、测试、部署与双语言分工。

## 课程流程索引

作业六步（选型 → 审计 → 文献 → 重构 → 报告 → 提交）的综述见：

`coursework/CPT304_Coursework/cpt304-coursework-01-complete-workflow.md`

不要把 **步骤 4 才要求的** i18n、Cookie 横幅、隐私页等误计为 **步骤 2 的源代码缺陷**；辨析见 `docs/defect-identification.md`。
