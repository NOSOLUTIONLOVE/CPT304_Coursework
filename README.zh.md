# CPT304 课程作业仓库

**语言：** [English](README.md) | 简体中文

## 项目概览

本仓库用于 **CPT304** 课程相关材料，并按四个顶层区域组织，便于快速定位：

- `coursework/`：课程流程、文档、模板与报告
- `projects/`：可写项目工作区（小组实际开发）
- `third_party/`：课程打包的第三方样本（只读对照）
- `artifacts/`：生成物（例如 Lighthouse JSON/HTML 导出）

## 仓库结构

```text
CPT304_Coursework/
├── README.md                    # 英文主说明
├── README.zh.md                 # 中文说明（本文件）
├── CODE_WIKI.md                 # 仓库内 Wiki / 导航笔记（若存在）
├── .gitignore
├── coursework/                  # 课程流程与交付材料（小组可读可写）
│   ├── README.md
│   ├── CPT304_Coursework/       # 官方指引副本、作业说明、端到端流程文档
│   └── docs/
│       ├── brainstorms/         # 需求 / 头脑风暴记录
│       ├── guides/              # 操作手册（如前置工作）
│       ├── plans/               # 带日期的实施或重构计划
│       ├── reports/             # 成稿报告（如 Lighthouse 评估）
│       └── templates/           # 可复用文档模板（如选型矩阵）
├── projects/                    # 小组实际开发的工作区
│   ├── README.md
│   └── inventory-management-system/
│       ├── README.md
│       ├── app/                 # 可运行的库存应用（HTML/JS/CSS + 资源；日常改代码主要在此）
│       └── docs/                # 仅针对该项目的文档
│           ├── baselines/       # 清单 / 「专业版」基线
│           ├── brainstorms/
│           ├── collaboration/   # 协作约定（如模块负责人）
│           ├── defects/         # 缺陷清单、审计、代码评审归档
│           ├── literature/    # 文献调研、引用、修复依据
│           ├── plans/
│           ├── setup/           # 开发环境与上手说明
│           ├── solutions/       # 修复记录、复盘式笔记
│           └── templates/
├── third_party/                 # 打包的上游样本（建议只读对照，慎作「长期主分支」修改）
│   ├── README.md
│   └── open_source_projects/    # 每个样本应用一个目录（见各自 README）
│       ├── README.md            # 总索引：各样本本地如何运行
│       ├── advanced-finance-tracker/
│       ├── biztrack/
│       ├── budget-app/
│       ├── e-commerce/
│       ├── employee-management-system/   # 前端 client + 后端 server
│       ├── inventory-app-js/            # Webpack / Tailwind 变体
│       ├── inventory-management-system/ # 基线应用的静态参考副本
│       ├── polln/                       # Django 风格全栈
│       ├── seat-booking-app/
│       └── taskify/                     # Node 应用（含 views、src 等）
└── artifacts/                   # 生成或导出的产物（非手写业务源码）
    ├── README.md
    └── lighthouse/
        └── <样本或项目名>/    # 每个应用一份 Lighthouse *.json + *.html
```

### 文件夹说明（速查）

| 路径 | 作用 |
|------|------|
| `coursework/` | 课程说明、计划、报告与团队共用模板的集中存放处。 |
| `coursework/CPT304_Coursework/` | 与 PDF 对齐的指引正文，以及浓缩后的作业全流程说明。 |
| `coursework/docs/` | 课程侧「活文档」：头脑风暴 → 计划 → 报告。 |
| `projects/` | 小组为完成作业而改代码的位置，避免污染 `third_party` 中的对照副本。 |
| `projects/.../app/` | 可部署或可本地运行的前端（或应用根目录），日常迭代的主目录。 |
| `projects/.../docs/` | 仅限该项目的规格、缺陷、文献与解决方案类文档。 |
| `third_party/open_source_projects/` | 随课打包的开源演示；作对照与 Lighthouse 基线参考。 |
| `artifacts/lighthouse/` | 按应用名保存的 Lighthouse 运行结果；JSON 为数据，HTML 为报告界面。 |

## 功能与内容

- **开源样本合集** — 纯前端、全栈（MongoDB/Express/React、Django/MySQL）及构建类示例（Webpack、Tailwind 等）。
- **本地运行说明** — macOS、Linux、Windows 下的安装与启动步骤见 `third_party/open_source_projects/README.md`。
- **课程材料** — Lighthouse 导出在 `artifacts/lighthouse/`；笔记、指南与报告在 `coursework/docs/`。
- **作业流程摘要** — 见 `coursework/CPT304_Coursework/cpt304-coursework-01-complete-workflow.md`（**以课程官方 PDF / Learning Mall 为准**）。

## 安装与克隆

1. 克隆仓库（SSH 示例）：

   ```bash
   git clone git@github.com:NOSOLUTIONLOVE/CPT304_Coursework.git
   cd CPT304_Coursework
   ```

2. 打开 `third_party/open_source_projects/README.md`，按所需子项目跳转到对应章节。多数静态项目仅需浏览器或 `python3 -m http.server`；需 Node 或 Python 的项目在同文件中说明了 `npm install`、虚拟环境或数据库等步骤。

## 使用方式

- **浏览课程材料**：从 `coursework/README.md` 与 `coursework/docs/` 开始。
- **小组主项目开发**：`projects/inventory-management-system/`。
- **运行样本项目**：按 `third_party/open_source_projects/README.md` 操作。
- **查看 Lighthouse 导出**：`artifacts/lighthouse/`。

## 许可证

除各子项目自带的 `LICENSE` 或上游许可另有规定外，本仓库中的课程整理材料仅用于 **CPT304 课程相关的教学与个人学习**。转载或修改各子项目代码时，请遵守其原始开源许可证。
