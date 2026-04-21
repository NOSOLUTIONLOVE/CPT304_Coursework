# Inventory Management System (UNIQLO) — 小组工作区

本目录是 CPT304 课题 **Inventory Management System (UNIQLO)** 在课程仓库内的**协作与文档根**，与课程打包的只读样本区分开，便于管理分工、缺陷、文献与「专业版」证据链。

## 官方课程列表与上游

- **列表中的名称：** Inventory Management System (UNIQLO)  
- **官方列表仓库（fork 源）：** https://github.com/sptin2002/inventory-management-system  
- **课程选型矩阵模板：** `docs/templates/cpt304-project-selection-matrix.md`（第 3 行）

将 **小组 fork 的 URL** 填在矩阵「最终决议」与下方（尚未创建 fork 时可暂缓）。

| 项目 | 内容 |
|------|------|
| 小组 fork URL | _待填写_ |
| 默认开发分支 | 建议 `main` / `develop`，组内统一即可 |

## 与 `Open_Source_Project/inventory-management-system` 的关系

`Open_Source_Project/inventory-management-system/` 是嵌入本仓库的**课程样本**，用于横向对比与本地快速打开，**一般不作为长期主开发分支**，除非你组明确选择以课程仓库为唯一提交物。

推荐做法：

1. **主开发在小组 fork**：日常 PR、Issue、部署预览均指向 fork。  
2. **本目录 `app/`**：说明与 fork 的同步方式（快照 / 子模块 / 仅链接）；见 `app/README.md`。  
3. 需要对照原版行为时，再打开 `Open_Source_Project` 中的副本。

## 目录结构

| 路径 | 用途 |
|------|------|
| `app/README.md` | 应用源码权威位置与同步策略 |
| `docs/dev-environment.md` | 开发环境与本地运行 |
| `docs/defect-identification.md` | 步骤 2：四座「真缺陷」识别方法 |
| `docs/templates/defect-log-template.md` | 缺陷登记模板 |
| `docs/literature-workflow.md` | 步骤 3：文献检索与引用流程 |
| `docs/templates/literature-matrix-template.md` | 缺陷–文献对应矩阵 |
| `docs/professional-edition-checklist.md` | 步骤 4：五项最低基准与证据占位 |
| `docs/module-owners.md` | 模块化功能域与负责人 |

## 模块化协作（概要）

按 **功能域** 并行，而非「每人随便改 `index.html`」，减少合并冲突。五域说明见 `docs/module-owners.md`，对应作业中的无障碍、测试、部署与双语言分工。

## 课程流程索引

作业六步（选型 → 审计 → 文献 → 重构 → 报告 → 提交）的综述见：

`CPT304_Coursework/cpt304-coursework-01-complete-workflow.md`

不要把 **步骤 4 才要求的** i18n、Cookie 横幅、隐私页等误计为 **步骤 2 的源代码缺陷**；辨析见 `docs/defect-identification.md`。
