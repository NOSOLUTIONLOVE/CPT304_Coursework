# `inventory-management-system/docs` 索引

文档按 **课程流程 + 类型** 分层组织，便于按步骤定位。

```
docs/
├── setup/             # 开发环境与本地运行
├── collaboration/     # 协作分工与约定
├── defects/           # 缺陷分析（步骤 2）
├── literature/        # 文献研究（步骤 3）
├── baselines/         # 基准清单（步骤 4）
├── templates/         # 登记表 / 矩阵模板
└── README.md          # 本文件
```

## setup — 开发环境

| 文档 | 用途 |
|------|------|
| [dev-environment.md](setup/dev-environment.md) | 本地 HTTP 服务器、可选 Node、调试入口 |

## collaboration — 协作管理

| 文档 | 用途 |
|------|------|
| [module-owners.md](collaboration/module-owners.md) | 模块化功能域、负责人与并行约定 |

## defects — 缺陷分析（步骤 2）

| 文档 | 用途 |
|------|------|
| [defect-identification.md](defects/defect-identification.md) | 步骤 2 合格/不合格缺陷标准与指南 |
| [source-defects-ranked.md](defects/source-defects-ranked.md) | 源码缺陷审查清单（按严重度 P1/P2/P3） |
| [step2-top4-defects.md](defects/step2-top4-defects.md) | 最终选定的 4 个缺陷（含复现步骤、根因、文献关键词） |
| [code-review-2026-04-27.md](defects/code-review-2026-04-27.md) | 人工源码审查记录（步骤 2 候选依据） |
| [compare-audit-his-vs-ours.md](defects/compare-audit-his-vs-ours.md) | 多来源缺陷分析对比（His / Ours / 组员 AI） |

## literature — 文献研究（步骤 3）

| 文档 | 用途 |
|------|------|
| [literature-workflow.md](literature/literature-workflow.md) | 文献检索策略、引用原则、与修复对应关系 |
| [search-queries.md](literature/search-queries.md) | 按缺陷排列的文献清单（DOI/URL） |
| [memory-leak-survey.md](literature/memory-leak-survey.md) | Web/JS 内存泄漏检测与修复综述（六篇论文） |
| [p1-1-remediation-from-survey.md](literature/p1-1-remediation-from-survey.md) | P1-1 缺陷从综述中可读出的修法与论证 |

## baselines — 五项最低基准（步骤 4）

| 文档 | 用途 |
|------|------|
| [professional-edition-checklist.md](baselines/professional-edition-checklist.md) | 五项基准状态追踪、风险与决策表 |

## templates — 模板

| 文档 | 用途 |
|------|------|
| [defect-log-template.md](templates/defect-log-template.md) | 缺陷登记表模板 |
| [literature-matrix-template.md](templates/literature-matrix-template.md) | 缺陷–文献对应矩阵模板 |

---

上级说明见 [`../README.md`](../README.md)。
