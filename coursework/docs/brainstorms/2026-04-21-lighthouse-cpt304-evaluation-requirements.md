---
date: 2026-04-21
topic: lighthouse-cpt304-multi-criteria-evaluation
---

# Lighthouse + CPT304：十项目多维度综合评估 — 需求说明

## Problem Frame

小组需在指定 10 个开源项目中选题并完成 CPT304 Coursework 1（见 `CPT304_Coursework/CPT304_Coursework_01_Details.md`）。仓库内已有基于 `Lighthouse/` JSON 的横向对比报告（`docs/reports/lighthouse-comparative-analysis.md`），但**未系统对照**评分维度（研究质量、基准标准、个人贡献）、**从原型到专业版**的工作量（5 项基准、4 缺陷修复链）、以及**环境配置**成本。需要一份**多维度综合评估**文档，辅助选题与工期风险判断，而非替代官方 PDF。

## Requirements

**数据来源**

- R1. 评估须显式引用 `docs/reports/lighthouse-comparative-analysis.md` 中的分类得分与 CWV 摘要，并说明数据来自 `Lighthouse/**/*.json`、本地单次采样的局限。
- R2. 须对照作业中的 **5 项最低基准**（在线 7 天、测试覆盖率 ≥80%、Lighthouse 无障碍 ≥90、双语言、Cookie+隐私页）逐项讨论**各项目类型的典型实现成本**（定性：低/中/高），不得声称某项目「已满足作业」——本仓库仅为本地副本与静态分析。

**评估维度（每组须有着落）**

- R3. **与评分标准对齐**：简述研究质量（50）、基准标准（30）、个人贡献（20）对各栈的含义（例如全栈需更多 PR 分工证据）。
- R4. **Lighthouse 与「专业版」差距**：以无障碍 ≥90 为核心锚点，结合当前 Lighthouse 无障碍分，给出**达标难度**判断；兼顾 Performance / SEO 是否暗示额外工程（如 EMS 开发态低分不等于生产不可达标）。
- R5. **环境配置难度**：依据 `Open_Source_Project/README.md` 中的依赖（MongoDB、MySQL、Python venv、Node 版本等）做**分级**（低/中/高），并标注跨平台注意点。
- R6. **缺陷审计与文献**：指出哪些类型项目更容易产生「4 个独立源码缺陷」叙述空间（与 comparative 报告中问题类型呼应），区分**缺陷**与**基准缺失**。

## Success Criteria

- 读者可为**至少 3 个项目**得到一致结论：「若选此题，主要风险在 X（测试 / 部署 / 无障碍 / 环境）」。
- 全文可追溯到 Lighthouse 数值或 README 运行说明，无无依据断言。

## Scope Boundaries

- 不指定「应该选谁」的唯一答案；允许列出相对更适合课程目标与相对更高风险的组合。
- 不修改 `Lighthouse/` 原始 JSON 或子项目源码。

## Key Decisions

- **交付物单文件**：`docs/reports/lighthouse-cpt304-multi-criteria-evaluation.md`，便于与现有 `lighthouse-comparative-analysis.md` 并列。
- **审查闭环**：成稿后执行文档自检（对应 `/ce:review` 的轻量替代：结构完整性与可追溯性）。

## Outstanding Questions

### Resolve Before Planning

- 无。

### Deferred to Planning

- 是否增加「推荐决策矩阵」表由规划确定。

## Next Steps

-> `/ce:plan` → `/ce:work` → `/ce:review`（自检 + 可选修订）
