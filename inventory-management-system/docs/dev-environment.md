# 开发环境安装与本地运行

课题栈（样本）：**纯静态前端** — HTML、Bootstrap、jQuery、DataTables、本地 JSON 文件 `DB.txt` 作为数据载体；无需数据库服务。

## 前置

- 现代浏览器（Chrome / Edge / Firefox / Safari 最新 stable 之一）。
- 文本编辑器（VS Code 推荐）。
- **可选：** Python 3 **或** Node.js（仅用于本地 HTTP，二选一即可）。

## 为什么尽量不用 `file://` 打开

直接用浏览器打开 `index.html`（`file://` 协议）可能导致脚本行为与通过 HTTP 服务时不一致，且不利于 Lighthouse 与部分安全策略复现。**请用本地 HTTP 服务**打开项目根目录。

## 方式 A：Python 内置服务器（无 Node）

在**应用根目录**（含 `index.html` 与 `assets/` 的目录）执行：

```bash
python3 -m http.server 5500
```

浏览器访问：`http://127.0.0.1:5500/`

停止服务：终端内 `Ctrl+C`。

## 方式 B：VS Code Live Server 插件

1. 安装扩展 **Live Server**。  
2. 在资源管理器中右键 `index.html` → **Open with Live Server**。  
3. 默认端口常为 `5500`，与课程已有 Lighthouse 路径习惯一致。

## 方式 C：Node（若已安装）

在应用根目录：

```bash
npx --yes serve -p 5500
```

## Node 何时「必需」

仅当组内为 **测试覆盖率** 或 **打包** 引入 `npm` 脚本（见 `professional-edition-checklist.md`）时才必须安装 Node；纯粹改静态文件完成课题未必需要。

## 无障碍与性能调试

- Chrome：**DevTools → Lighthouse**（与课程步骤一致）。  
- **DevTools → Elements / Accessibility** 树：检查 landmark、`lang`、名称与对比度（支撑无障碍基准）。

## 与课程样本路径对齐

若要复现实验室已有报告的条件，可对 **课程样本** 启动服务：

```bash
cd Open_Source_Project/inventory-management-system
python3 -m http.server 5500
```

你 fork 后的仓库结构应与此类似；若你组改了根路径，相应更新 Lighthouse 的 `requestedUrl`。
