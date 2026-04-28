---
date: 2026-04-21
topic: lighthouse-analysis-report
---

# Lighthouse 多项目对比分析报告 — 需求说明

## Problem Frame

课程作业在 `Lighthouse/` 目录下已为十个开源子项目各保存了一次 Lighthouse（导航模式）的 JSON 与 HTML 报告。读者需要一份**独立于原始 HTML 的、可阅读的对比分析**，把四类分类得分（Performance / Accessibility / Best Practices / SEO）与关键实验室指标（FCP、LCP 等）汇总到同一视图中，并解释**差异来源**（技术栈、页面复杂度、本地运行环境），而不是只罗列分数。

## Requirements

**内容与结构**

- R1. 报告须基于 `Lighthouse/` 中已有产物撰写；不强制要求重新运行 Lighthouse，但若引用数据须标明来源文件路径与抓取时间（来自 JSON 的 `fetchTime` / `lighthouseVersion`）。
- R2. 提供**总览对比表**：每个子项目一行，至少包含四类分类得分（0–100 或 Lighthouse 原始 0–1 换算为百分制）、测试 URL、以及可选的核心 Web 指标摘要（FCP、LCP、TBT、CLS）。
- R3. 按项目类型（纯静态、构建后静态、Vite SPA、Django、Node/EJS 等）做**简短分组或标注**，避免把不可比的 URL（例如 `localhost:5173` 与静态 `index.html`）误读为「同一基准」。
- R4. 对得分明显偏低的维度，结合 JSON 中**未通过的审计项标题**做定性说明（不要求穷尽所有审计，但需覆盖主要短板）。
- R5. 单独一节说明**方法局限**：本地 HTTP、非 HTTPS、单次采样、无障碍仅自动化检测等。

**读者与语言**

- R6. 正文使用**简体中文**，专业名词可保留英文缩写（LCP、CLS、Lighthouse 等）。

## Success Criteria

- 读者可在 5 分钟内从报告中回答：「哪个项目在哪一维最弱？」「静态站与全栈应用在性能上的典型差异是什么？」
- 任意分数或指标均能在 `Lighthouse/<项目名>/` 下找到对应 JSON 佐证。

## Scope Boundaries

- 不修改各子项目源代码以「刷分」；本交付物为**分析报告文档**。
- 不包含对上游开源仓库维护者的 issue 或 PR 行动项（可在文末以「可选改进方向」一句话带过）。

## Key Decisions

- **单一交付文件**：主报告放在 `docs/reports/`，便于与课程说明、计划文档区分。
- **数据以 JSON 为准**：HTML 报告用于人工核对，汇总表以解析 JSON 得到的数值为准。

## Dependencies / Assumptions

- 假设 `Lighthouse/` 下十个目录与 `Open_Source_Project/README.md` 中的十个项目一一对应。
- 本地端口与路径（如 `127.0.0.1:5500`）反映当时实验环境，分析中视为一致前提而非缺陷。

## Outstanding Questions

### Resolve Before Planning

- （无 — 可直接进入规划。）

### Deferred to Planning

- 报告文件名与是否增加「附录：原始文件清单」由规划阶段定稿。

## Next Steps

-> `/ce:plan`：产出整理步骤与文件路径；再由 `/ce:work` 生成 `docs/reports/` 下的最终 Markdown 报告。
