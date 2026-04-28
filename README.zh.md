# CPT304 课程作业仓库

**语言：** [English](README.md) | 简体中文

## 项目概览

本仓库用于 **CPT304** 课程相关材料，并按四个顶层区域组织，便于快速定位：

- `coursework/`：课程流程、文档、模板与报告
- `projects/`：可写项目工作区（小组实际开发）
- `third_party/`：课程打包的第三方样本（只读对照）
- `artifacts/`：生成物（例如 Lighthouse JSON/HTML 导出）

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
