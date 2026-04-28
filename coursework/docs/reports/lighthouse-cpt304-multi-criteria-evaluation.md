# 基于 Lighthouse 的 CPT304 十项目多维度综合评估

**日期：** 2026-04-21  
**依据：** `coursework/docs/reports/lighthouse-comparative-analysis.md`（数据源自 `artifacts/lighthouse/**/*.json`）、`coursework/CPT304_Coursework/CPT304_Coursework_01_Details.md`、`third_party/open_source_projects/README.md`。

**声明：** 本文用于**选题与工期风险**讨论，不替代课程官方 PDF；未在生产环境验证部署与覆盖率，所有「难度」均为**定性判断**。

---

## 1. 摘要

在本地单次 Lighthouse（导航模式、HTTP）条件下，**静态 HTML 类项目**普遍 Performance 更高，但 **Accessibility 分化大**：`inventory-app-js`、`e-commerce` 实验室无障碍分显著偏低，意味着要达到作业要求的 **Lighthouse 无障碍 ≥90** 往往需要**更多语义化与表单改造**。全栈类（**employee-management-system**、**polln**）环境依赖更重（MongoDB / MySQL），**从可运行到可部署、可测到 80% 覆盖率**，整体工程面更宽。本文将 Lighthouse 结果与 **CPT304 三维评分**、**5 项最低基准**、**环境配置**一并对齐，便于组内权衡。

---

## 2. 课程要求与本文角色（「老师的要求」速览）

| 课程要素 | 内容摘要 | 与 Lighthouse 的关系 |
|----------|----------|---------------------|
| 研究质量（50 分） | 4 个**源码缺陷** + 工具检测 + 4 篇文献 + 修复前后代码 + 逻辑链 | Lighthouse/axe 是**检测手段之一**；缺陷不能混同「未做 i18n」等基准缺失 |
| 基准标准（30 分） | 第 6 章截图：**7 天在线、覆盖率 ≥80%、无障碍 ≥90、双语言、Cookie+隐私页** | 本文「无障碍」列与作业阈值 **90** 直接对齐；其余项需部署/CI，非 Lighthouse 单机一次能证 |
| 个人贡献（20 分） | 贡献表、Git 记录、**PR 链接** | 全栈/多包仓库通常分支与 PR 面更大，需提前分工 |
| 交付物 | `GroupXX.zip`：report、GitHub、**live URL**、互评表 | 静态站易上 Vercel；Django/Node 需配置构建与路由 |

---

## 3. 评估维度说明

| 维度 | 含义 | 主要信息源 |
|------|------|------------|
| **A. Lighthouse 现状** | Performance / Accessibility / Best Practices / SEO 及 CWV | `lighthouse-comparative-analysis.md` |
| **B. 无障碍达标压力** | 当前无障碍分与 **90** 的差值及典型问题类型 | 同上 + 分项观察 |
| **C. 环境配置难度** | 依赖种类、数据库、多进程、跨平台 | `Open_Source_Project/README.md` |
| **D. 原型→专业版（作业口径）** | 5 项基准在各类栈上的**典型工作量**（非分数） | 作业 PDF + 栈类型 |
| **E. 叙事与审计空间** | 是否容易找到 **4 个独立缺陷**且与文献匹配 | 易混淆点 + 报告结构 |

---

## 4. 五项最低基准 × 技术栈：典型难度（定性）

| 基准 | 静态多页 HTML | 前端构建（Webpack 等） | SPA（Vite/React） | Django | Node/EJS |
|------|---------------|-------------------------|-------------------|--------|----------|
| 7 天在线 | 通常低 | 中（需正确 build 输出） | 中–高 | 中–高 | 中 |
| 测试 ≥80% | 往往**高**（需补测试基建） | 中–高 | 中（现成工具链多） | 中 | 中 |
| 无障碍 ≥90 | 视当前分而定 | 视当前分 | 中（组件多） | 中 | 中 |
| 双语言 i18n | 中（字符串分散） | 中 | 中–高 | 中（框架支持） | 中 |
| Cookie + 隐私页 | 低–中 | 中 | 中 | 中 | 中 |

---

## 5. Lighthouse 数据与作业阈值对照（摘自对比报告）

无障碍 **≥90** 为作业硬阈值。下表为各项目 **Accessibility** 实验室分（百分制），第三列为 **（当前分 − 90）**：**正值**表示已超过阈值，**负值**表示尚未达标（绝对值越大通常压力越大）。

| 项目（目录名） | A11y | 当前分 − 90 | 备注（来自对比报告） |
|----------------|-----:|--------------:|----------------------|
| advanced-finance-tracker | 100 | +10 | 已超过阈值 |
| budget-app | 96 | +6 | 已超过阈值 |
| biztrack | 88 | −2 | 略低于阈值 |
| polln | 88 | −2 | 略低于阈值 |
| employee-management-system | 80 | −10 | 明显低于阈值 |
| taskify | 83 | −7 | 低于阈值 |
| inventory-management-system | 86 | −4 | 低于阈值 |
| seat-booking-app | 78 | −12 | 明显低于阈值 |
| e-commerce | 74 | −16 | 明显低于阈值 |
| inventory-app-js | 63 | −27 | 当前最低 |

**Performance** 与 **SEO** 不是作业单独列分的数字阈值，但 **SEO 低**常伴随 `alt`、标题结构等问题，与无障碍、第 6 章截图叙事相关；**EMS** 在开发态 Performance 低，**生产构建与部署后**可能显著改善，不宜单独作为「不能达标」依据。

---

## 6. 逐项目综合评估（多维）

### 6.1 advanced-finance-tracker（纯静态）

- **Lighthouse：** 全面高分；无障碍已 100。  
- **环境：** 低（浏览器或 `python -m http.server`）。  
- **5 项基准：** 部署与在线较顺；**测试覆盖率 80%** 需从零引入测试框架与用例，往往是主要工时。  
- **课程叙事：** 需在 HTML/JS 中找够 4 个**真实缺陷**（非「未国际化」），避免与基准混谈。

### 6.2 biztrack（纯静态）

- **Lighthouse：** A11y 88，差 2 分达 90。  
- **环境：** 低。  
- **专业版路径：** 无障碍小步修补即可；覆盖率与 i18n 仍要统一规划。

### 6.3 budget-app（纯静态）

- **Lighthouse：** A11y 96，已超阈值。  
- **环境：** 低。  
- **风险点：** 作业亮点更多依赖**文献质量**与**缺陷是否具体**，而非 Lighthouse 分差。

### 6.4 e-commerce（静态 + Bootstrap）

- **Lighthouse：** A11y 74、Best 88；问题含对比度、无 `alt`、链接名称等。  
- **环境：** 低。  
- **专业版路径：** 无障碍与 SEO 需**成体系改**；控制台错误需消。适合作为「可写满 4 章缺陷分析」的样本，但**修复+基准**工作量大。

### 6.5 employee-management-system（Mongo + Express + Vite/React）

- **Lighthouse：** Performance 65、A11y 80、SEO 75；LCP 实验室偏高（开发态）。  
- **环境：** **高**（MongoDB、前后端双 `npm`、两终端）。  
- **专业版路径：** 测试与 CI 面相对友好（现代前端工具链）；**部署**与**7 天稳定**需认真配置；无障碍与性能建议在**生产 build** 上复测 Lighthouse。

### 6.6 inventory-app-js（Webpack + Tailwind）

- **Lighthouse：** A11y **63** 为全表最低；表单/标题/地标问题多。  
- **环境：** 中（`npm install`、构建链）。  
- **专业版路径：** **无障碍到 90** 可能占用大量迭代；覆盖率需适配现有结构。

### 6.7 inventory-management-system（jQuery + Bootstrap + DataTables）

- **Lighthouse：** Performance 66、A11y 86；Legacy JS、阻塞请求等。  
- **环境：** 低–中（静态为主）。  
- **专业版路径：** 性能与无障碍需处理**遗留脚本**；测试覆盖率需专门设计。

### 6.8 polln（Django + MySQL）

- **Lighthouse：** 整体较好，A11y 88。  
- **环境：** **高**（Python venv、`mysqlclient`、迁移、数据库）。  
- **专业版路径：** 部署与生产配置复杂；Django 对 i18n、独立页面较友好；**全栈测试**与覆盖率需覆盖后端逻辑。

### 6.9 seat-booking-app（静态，可选 Sass）

- **Lighthouse：** A11y 78。  
- **环境：** 低–中。  
- **专业版路径：** 无障碍需一轮；其余同静态类。

### 6.10 taskify（Node + Express + EJS）

- **Lighthouse：** A11y 83、Best 92、SEO 82；图片比例/分辨率问题。  
- **环境：** 中（Node、`.env`、可选 Mongo）。  
- **专业版路径：** 服务端渲染站常见路由+模板改造；测试需覆盖路由与模板逻辑。

---

## 7. 横向决策矩阵（选题速查）

**图例：** 环境：低 / 中 / 高。无障碍压力：与 90 的差距（越大压力越高）。综合工程面：主观**低 / 中 / 高**（含 5 基准 + 环境 + 全栈广度）。

| 项目 | 环境配置 | A11y 压力 | Performance 实验室 | 综合工程面（作业） | 一句话策略 |
|------|----------|-----------|---------------------|-------------------|------------|
| advanced-finance-tracker | 低 | 极低 | 优 | **中**（测覆盖率） | 易跑、易部署；主攻测试与叙事 |
| biztrack | 低 | 低 | 优 | 中 | 小修无障碍 + 基准套餐 |
| budget-app | 低 | 极低 | 优 | 中 | 分数好看；突出缺陷与文献深度 |
| e-commerce | 低 | **高** | 良 | **高** | 缺陷多、改面大；适合能投入时间的组 |
| employee-management-system | **高** | 高 | 差（dev） | **高** | 全栈分工；生产态复测 |
| inventory-app-js | 中 | **很高** | 良 | **高** | 除非擅长 a11y，否则谨慎 |
| inventory-management-system | 低–中 | 中 | 差 | **高** | 遗留 JS + 性能 + 测试 |
| polln | **高** | 低 | 良 | **高** | 环境重；适合熟悉 Python/SQL 的组 |
| seat-booking-app | 低–中 | 高 | 优 | 中–高 | 静态中修复面较大 |
| taskify | 中 | 中 | 优 | 中–高 | SSR + 图片与合规；部署需设计 |

---

## 8. 风险与选题策略（非排名）

- **追求「Lighthouse 报表好看」：** `advanced-finance-tracker`、`budget-app` 当前实验室分数最高，但**不代表**报告 50 分自动拿满——仍看缺陷与文献。  
- **追求「缺陷可写、可引文献」：** `e-commerce`、`inventory-app-js` 问题类型丰富，但**修复与达标工作量**大。  
- **追求「全栈与部署故事」：** `employee-management-system`、`polln` 更接近「系统集成」学习成果，但**环境与截止时间**压力大。  
- **平衡组内技能：** 熟悉 **Node** 可偏向 EMS、taskify、biztrack 等；熟悉 **Python** 可评估 polln；**仅前端**且时间紧可评估静态项目中 **A11y 压力低**者。

---

## 9. 来源、局限与可追溯性

- **Lighthouse：** 见 `coursework/docs/reports/lighthouse-comparative-analysis.md` 第 2、3、6 节及原始 JSON 路径列表。
- **课程条文：** 以 `coursework/CPT304_Coursework/CPT304_Coursework_01_Details.md` 为准。
- **局限：** 单次采样、本地 HTTP、开发服务器与生产不一致；无障碍为自动化规则。

---

## 10. 审查摘要（对应 `/ce:review` 轻量自检）

**审查范围：** 本文档结构与可追溯性（非代码 diff）。

| 检查项 | 结果 |
|--------|------|
| 是否与 `lighthouse-comparative-analysis.md` 数值一致 | 已对齐无障碍分与分项叙述 |
| 是否区分「缺陷」与「5 项基准」 | 第 2、6 节已强调 |
| 是否避免「官方未规定」的硬指标 | Performance/SEO 仅作辅助，作业阈值仅明确引用无障碍 ≥90 |
| 局限与声明 | 第 9 节 + 文首声明 |
| 残留假设 | 生产构建未实测；覆盖率与部署未验证 |

**结论：** 可作为内部选题讨论稿发布；若用于对外提交，需按课程要求独立撰写 **report.pdf**，不得直接粘贴本文。

---

## 11. 选题推荐排序（第 1～10 名）

**假设：** 一般 4 人小组、优先降低挂科与延期风险、以本文第 5～8 节综合评估为主要依据。**第 1 名 = 最推荐，第 10 名 = 最不推荐。**

| 排名 | 项目 | 简要理由 |
|------|------|----------|
| **1** | **budget-app** | 实验室无障碍 **96**，距作业 **≥90** 压力小；纯静态、环境成本低；精力可更多放在 **80% 覆盖率、i18n、部署与报告**。 |
| **2** | **advanced-finance-tracker** | 无障碍 **100**；与第 1 名同属「Lighthouse 易达标、测试覆盖率仍是主战场」。 |
| **3** | **biztrack** | 无障碍 **88**，约差 **2 分**到 90；环境仍低，整体风险可控。 |
| **4** | **polln** | 无障碍 **88**；环境 **重**（Python、MySQL、迁移），但 Django 对 **i18n、独立页面**往往较顺。组内 **熟 Python/SQL** 时，实际体感可接近前三。 |
| **5** | **taskify** | 无障碍 **83**，尚属可专项补上；环境中等；比纯静态多服务端与路由，工程面更大。 |
| **6** | **seat-booking-app** | 环境相对友好；无障碍 **78**，爬升到 90 的工作量明显大于第 3、4 名。 |
| **7** | **inventory-management-system** | 无障碍 **86**；另有 Legacy JS、性能实验室分偏低，测试与重构面更杂。 |
| **8** | **employee-management-system** | 全栈 + Mongo + Vite；无障碍 **80**，开发态 Performance 偏低（生产构建后可能改善）；**分工与 PR**好写，整体工期与集成风险偏高。 |
| **9** | **e-commerce** | 无障碍 **74**，缺陷叙事丰富，但 **修复 + 5 项基准**总工作量大，对「稳妥过关」不友好。 |
| **10** | **inventory-app-js** | 无障碍 **63** 为十项中最低，达标 **90** 的工程量通常最大；除非组内 **a11y/前端重构**很强，否则一般不优先。 |

**微调提示：** 若组内 **Python/Django + MySQL 很熟**，**polln** 可上调至 **第 2～3 名**；若优先 **报告故事性、缺陷可写**，**e-commerce** 可上调，但需接受工期上升。最终以课程 PDF 与 fork 后真实仓库为准。

---

*需求：`coursework/docs/brainstorms/2026-04-21-lighthouse-cpt304-evaluation-requirements.md` · 计划：`coursework/docs/plans/2026-04-21-003-feat-lighthouse-cpt304-evaluation-plan.md`*
