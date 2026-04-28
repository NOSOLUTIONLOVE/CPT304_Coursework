---
date: 2026-04-28
topic: repo-structure-navigation
---

# CPT304：课程大仓结构重整（导航优先）

## Problem Frame

当前仓库同时承载：

- 课程大仓材料（流程、模板、报告）
- 小组可写开发工作区（`inventory-management-system/`）
- 十个开源样本副本（`Open_Source_Project/`）
- 质量审计产物（`Lighthouse/`）

但入口分散，读者需要记忆“去哪找什么”，且容易混淆 **样本区** 与 **主开发区** 的边界，导致导航成本高、误改风险大。

## Goals

- G1. **30 秒内定位**：新加入组员或助教能从根目录快速找到“课程材料 / 我们开发的项目 / 只读样本 / 审计产物”四类内容入口。
- G2. **边界清晰**：目录语义能自然阻止误用（例如把样本当主开发）。
- G3. **低维护成本**：不依赖复杂站点生成；通过目录命名 + 入口文件 + 规则即可持续维护。

## Proposed Top-level Layout（方案 A，目标结构）

在仓库根目录形成四大区域：

- `coursework/`：课程流程、通用模板、课程说明、写作指南等“与具体项目无关”的材料。
- `projects/`：本组（或本仓）**可写**的项目工作区（包含源码、项目级文档、证据链）。
- `third_party/`：课程打包的 10 个开源项目副本（只读对照/参考），以及任何上游快照。
- `artifacts/`：审计/截图/导出文件等“生成物”（例如 Lighthouse 报告 JSON/HTML）。

## Requirements

**R1. 根入口**

- 根 `README.md` 必须以“我是来找什么的？”为导向提供 4 个入口：
  - 课程材料入口（`coursework/`）
  - 小组项目入口（`projects/`，并点名主项目）
  - 只读样本入口（`third_party/`，明确不可作为主开发）
  - 生成物入口（`artifacts/`，说明其可再生成）

**R2. 单一事实来源（SSOT）声明**

- 每个 `projects/<project>/README.md` 必须声明：
  - **主开发位置**（本仓库路径 / 或 GitHub fork 作为权威）
  - 与 `third_party/` 中同名样本/快照的关系（仅对照/同步策略）

**R3. 目录映射规则（从现状迁移到目标结构）**

- `docs/`（当前）→ `coursework/docs/`（或按子类拆分到 `coursework/` 内）
- `inventory-management-system/`（当前）→ `projects/inventory-management-system/`
- `Open_Source_Project/`（当前）→ `third_party/open_source_projects/`
- `Lighthouse/`（当前）→ `artifacts/lighthouse/`

（具体是否保留旧路径的兼容层，由后续计划决定；但最终目标结构必须满足 R1/R2。）

**R4. 导航索引文件**

- `coursework/`、`projects/`、`third_party/`、`artifacts/` 各自必须有一个 `README.md` 作为目录索引，至少包含：
  - 该目录的“用途一句话”
  - 关键子目录链接（3–10 条，避免变成目录树垃圾场）
  - 使用/更新规则（例如：`artifacts/` 可再生，不要手改）

**R5. 命名与一致性**

- 目录与文件命名统一使用 `kebab-case`（或保持既有风格，但需在根 README 明确标准）。
- 避免同名不同义（例如两个 `inventory-management-system` 同时存在且都像“主项目”）。

**R6. Git hygiene（为导航服务）**

- 仓库必须避免引入与导航无关的噪音文件进入版本控制（例如 `.DS_Store`）。
- 编辑器/工具配置若需要提交，必须放在明确的位置并说明用途（例如 `.cursor/` 仅包含必要共享配置）。

## Success Criteria

- S1. 任何读者从根 `README.md` 出发，**不搜索**也能在 30 秒内找到：
  - CPT304 流程/模板所在
  - 组内主项目源码与其文档
  - 十个样本项目的运行说明
  - Lighthouse 等审计产物
- S2. `projects/` 与 `third_party/` 的边界通过命名与 README 文字“强约束”，不再需要口头解释。

## Scope Boundaries

- 不引入文档站点生成器（如 MkDocs/Docusaurus）作为前提条件；保持可直接在 GitHub 阅读。
- 不在本 requirements 中规定具体文件拆分/重构代码方式（那是规划与实现阶段）。

## Outstanding Questions

- Q1. 是否需要保留旧目录作为兼容入口（例如保留 `Lighthouse/` 但仅放一个指向 `artifacts/lighthouse/` 的说明）？
- Q2. `projects/` 下是否只放本组课题，还是未来还会把其它练习项目也搬入？

## Next Steps

- 将本需求交给 `/ce-plan`：产出“迁移计划 + 验收 checklist + 最小可回滚策略”。
