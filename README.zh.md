# CPT304 课程作业仓库

**语言：** [English](README.md) | 简体中文

## 项目概览

本仓库用于 **CPT304** 课程相关材料。主体内容在 `Open_Source_Project/` 目录：内含 **10 个开源项目** 的本地副本（静态页、Node/Express、Django 及混合技术栈），各子项目有独立运行说明。`Lighthouse/` 目录存放网页审计产物（JSON/HTML，例如各项目的 Lighthouse 报告）。

## 功能与内容

- **开源样本合集** — 纯前端、全栈（MongoDB/Express/React、Django/MySQL）及构建类示例（Webpack、Tailwind 等）。
- **本地运行说明** — macOS、Linux、Windows 下的安装与启动步骤见 `Open_Source_Project/README.md`。
- **课程材料** — `Lighthouse/` 为审计数据；`docs/` 为笔记、指南与报告（如 `docs/guides/`、`docs/reports/`）。
- **作业流程摘要** — 见 `CPT304_Coursework/cpt304-coursework-01-complete-workflow.md`（**以课程官方 PDF / Learning Mall 为准**）。

## 安装与克隆

1. 克隆仓库（SSH 示例）：

   ```bash
   git clone git@github.com:NOSOLUTIONLOVE/CPT304_Coursework.git
   cd CPT304_Coursework
   ```

2. 打开 `Open_Source_Project/README.md`，按所需子项目跳转到对应章节。多数静态项目仅需浏览器或 `python3 -m http.server`；需 Node 或 Python 的项目在同文件中说明了 `npm install`、虚拟环境或数据库等步骤。

## 使用方式

- **浏览代码与作业**：在 `Open_Source_Project/` 及各子目录中查看。
- **运行某个应用**：严格按 `Open_Source_Project/README.md` 中该项目的说明操作（含路径、`.env`、端口等）。
- **查看审计文件**：按需打开 `Lighthouse/` 下的 `.html` 或 `.json`。
- **扩展阅读**：可选阅读 `docs/guides/cpt304-prerequisite-work-playbook.md`、`docs/reports/` 中的 Lighthouse 与 CPT304 评估笔记。

## 许可证

除各子项目自带的 `LICENSE` 或上游许可另有规定外，本仓库中的课程整理材料仅用于 **CPT304 课程相关的教学与个人学习**。转载或修改各子项目代码时，请遵守其原始开源许可证。
