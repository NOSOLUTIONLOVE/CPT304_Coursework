# Lighthouse 多开源项目对比分析报告

## 1. 摘要

本报告基于仓库内 `Lighthouse/` 目录下 **10 个子项目**各一份 Lighthouse **导航模式（navigation）** JSON 报告，对 **Performance、Accessibility、Best Practices、SEO** 四类得分及核心 Web 指标（FCP、LCP、TBT、CLS）进行对比。整体而言：**纯静态或通过轻量服务打开的 HTML 页面性能分数普遍较高**；**Vite 开发服务器上的 React 客户端与部分依赖 jQuery/Bootstrap 的静态页**在性能或无障碍上出现明显短板。以下为数据驱动的归纳，详细数值见第 3 节总表。

## 2. 方法与数据来源

| 项目 | 说明 |
|------|------|
| 工具版本 | 各 JSON 中 `lighthouseVersion` 均为 **13.0.2**（见各文件顶部字段）。 |
| 采样时间 | 各次运行 `fetchTime` 在 **2026-04-21T05:23Z–05:55Z（UTC）** 区间内，属同一工作会话内连续测试；非同一时刻的横向对比仍具参考价值，但不宜视为严格控制变量的实验。 |
| 数据文件 | 每个子项目在 `Lighthouse/<项目名>/` 下均有 `.json` 与 `.html`；**本报告数值以 `.json` 为准**，HTML 便于对照可视化。 |
| 环境 | 均为本地 URL（`127.0.0.1`、`localhost`），**非 HTTPS**；Lighthouse 对安全与 PWA 相关项的解读需结合此背景。 |
| 局限 | **单次采样**；无障碍为**自动化规则**，不能替代人工审查；本地 CPU/网络负载会影响性能分数。 |

与 `Open_Source_Project/README.md` 中的技术栈分类对齐，下表「类型」列便于理解**不可直接横向等同**的页面（例如 Vite 开发态与静态 `index.html`）。

## 3. 总览对比表

### 3.1 分类得分（百分制）

将 JSON 中 `categories.<id>.score`（0–1）换算为 **0–100** 整数。

| 子项目 | 类型（见 Open_Source 说明） | Performance | Accessibility | Best Practices | SEO |
|--------|----------------------------|------------:|--------------:|---------------:|----:|
| advanced-finance-tracker | 纯前端静态 | 100 | 100 | 100 | 90 |
| biztrack | 纯前端静态 | 100 | 88 | 100 | 90 |
| budget-app | 纯前端静态 | 100 | 96 | 100 | 90 |
| e-commerce | 纯前端静态 | 97 | 74 | 88 | 91 |
| employee-management-system | 全栈（Vite 前端） | 65 | 80 | 100 | 75 |
| inventory-app-js | 前端构建（预览 public） | 99 | 63 | 100 | 90 |
| inventory-management-system | 纯前端静态 | 66 | 86 | 100 | 82 |
| polln | 全栈 Django | 97 | 88 | 100 | 91 |
| seat-booking-app | 纯前端静态 | 100 | 78 | 100 | 90 |
| taskify | Node / Express / EJS | 100 | 83 | 92 | 82 |

**数据来源路径示例**：`Lighthouse/advanced-finance-tracker/127.0.0.1_5500-20260421T132304.json`（其余项目见同目录命名规则）。

### 3.2 核心 Web 指标（来自各 JSON `audits`）

| 子项目 | FCP | LCP | TBT | CLS |
|--------|-----|-----|-----|-----|
| advanced-finance-tracker | 0.3 s | 0.3 s | 0 ms | 0 |
| biztrack | 0.6 s | 0.6 s | 0 ms | 0.024 |
| budget-app | 0.3 s | 0.4 s | 0 ms | 0.039 |
| e-commerce | 0.8 s | 1.2 s | 0 ms | 0.003 |
| employee-management-system | 2.4 s | 4.1 s | 20 ms | 0 |
| inventory-app-js | 0.7 s | 0.7 s | 0 ms | 0.001 |
| inventory-management-system | 2.8 s | 3.2 s | 20 ms | 0.023 |
| polln | 0.5 s | 1.3 s | 0 ms | 0.001 |
| seat-booking-app | 0.3 s | 0.3 s | 0 ms | 0 |
| taskify | 0.5 s | 0.5 s | 0 ms | 0.001 |

## 4. 分维度观察

### 4.1 Performance（性能）

- **高分集群（97–100）**：多数静态页与 taskify、polln 首页在此维度表现很好；实验室环境下 FCP/LCP 多在 1 s 内或略超。
- **明显偏低**：
  - **employee-management-system（65）**：`first-contentful-paint` / `largest-contentful-paint` 等实验室指标显著变差（FCP 约 2.4 s、LCP 约 4.1 s），与 **Vite 开发服务器 + React 打包体积**一致；JSON 中常见未满分项包括 **Render-blocking requests**、**Reduce unused JavaScript**、**Largest Contentful Paint**、**bfache** 相关提示等。
  - **inventory-management-system（66）**：LCP 约 3.2 s，与 **Legacy JavaScript**、**Render-blocking requests**、**Reduce unused CSS/JavaScript** 等审计项在 JSON 中未通过一致。
- **结论**：性能短板高度集中于 **重 JS 管线（EMS 客户端）** 与 **旧式前端依赖链（inventory-management-system）**，而非「静态 HTML 本身慢」。

### 4.2 Accessibility（无障碍）

- **最高**：advanced-finance-tracker（100）。
- **需关注（≤86）**：e-commerce（74）、inventory-app-js（63）、seat-booking-app（78）、biztrack（88）、polln（88）等。
- **典型未通过审计（摘自 JSON）**：
  - **e-commerce**：对比度不足、`img` 缺 `alt`、`aria-hidden` 内仍含可聚焦元素、链接无可辨识名称等。
  - **inventory-app-js**：按钮缺可访问名称、标题层级乱序、表单控件无关联 `label`、缺 `main` landmark 等。
  - **employee-management-system**：图片缺 `alt`、文档缺 **main landmark**。
  - **inventory-management-system**：`<html>` 缺 `lang`、图片缺 `alt`、缺 **main landmark**。

### 4.3 Best Practices（最佳实践）

- 多数项目为 **100**。
- **e-commerce（88）**：除图片宽高比/分辨率问题外，JSON 中可见 **Browser errors were logged to the console**。
- **taskify（92）**：**Displays images with incorrect aspect ratio**、**Serves images with low resolution** 等。

### 4.4 SEO

- 静态站多为 **90** 左右；**employee-management-system（75）** 与 **inventory-management-system（82）**、**taskify（82）** 相对较低。
- **e-commerce** 中 SEO 与无障碍重叠的问题包括 **图片缺少 `alt`**（同时影响 SEO 与 a11y）。

## 5. 综合结论

1. **静态资源型页面**在 Performance 上普遍更「好看」，但无障碍与 SEO 仍可能因 **语义 HTML、对比度、替代文本** 而失分。
2. **现代前端构建链（EMS + Vite）**在开发态下容易暴露 **性能与 SEO** 压力，需在课程结论中区分「开发服务器体验」与「生产构建优化后」的差异（本仓库未测生产构建）。
3. **inventory-app-js** 与 **e-commerce** 在 **Accessibility** 上问题清单最长，适合作为「改进语义与表单」的教学案例。
4. 所有结论均受 **第 2 节方法局限**约束；若需提交正式课程报告，建议声明测试环境与单次采样。

## 6. 原始文件索引

| 子项目 | JSON（用于本报告） |
|--------|-------------------|
| advanced-finance-tracker | `Lighthouse/advanced-finance-tracker/127.0.0.1_5500-20260421T132304.json` |
| biztrack | `Lighthouse/biztrack/127.0.0.1_5500-20260421T132415.json` |
| budget-app | `Lighthouse/budget-app/127.0.0.1_5500-20260421T132545.json` |
| e-commerce | `Lighthouse/e-commerce/127.0.0.1_5500-20260421T132808.json` |
| employee-management-system | `Lighthouse/employee-management-system/localhost_5173-20260421T133831.json` |
| inventory-app-js | `Lighthouse/inventory-app-js/127.0.0.1_5500-20260421T134133.json` |
| inventory-management-system | `Lighthouse/inventory-management-system/127.0.0.1_5500-20260421T134214.json` |
| polln | `Lighthouse/polln/127.0.0.1_8000-20260421T135057.json` |
| seat-booking-app | `Lighthouse/seat-booking-app/127.0.0.1_5500-20260421T135224.json` |
| taskify | `Lighthouse/taskify/127.0.0.1_3000-20260421T135543.json` |

---

*文档生成说明：与 `docs/plans/2026-04-21-001-feat-lighthouse-comparative-report-plan.md` 对齐；需求见 `docs/brainstorms/2026-04-21-lighthouse-analysis-report-requirements.md`。*
