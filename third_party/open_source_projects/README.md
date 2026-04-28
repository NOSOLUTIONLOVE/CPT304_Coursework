# open_source_projects 本地运行说明

本目录包含 **10 个开源项目** 的副本。下表为技术栈概览，后文按项目给出安装环境、终端命令与 **macOS / Windows** 差异说明。

| 项目 | 类型 | 主要技术 |
|------|------|----------|
| [advanced-finance-tracker](#1-advanced-finance-tracker) | 纯前端静态 | HTML / CSS / JavaScript |
| [biztrack](#2-biztrack) | 纯前端静态 | HTML / CSS / JavaScript |
| [budget-app](#3-budget-app) | 纯前端静态 | HTML / CSS / JavaScript |
| [e-commerce](#4-e-commerce) | 纯前端静态 | Vanilla JS、Bootstrap、预编译样式 |
| [employee-management-system](#5-employee-management-system) | 全栈 | MongoDB、Express、React (Vite)、Node |
| [inventory-app-js](#6-inventory-app-js) | 前端构建 | Webpack、Babel、Tailwind（需 Node） |
| [inventory-management-system](#7-inventory-management-system) | 纯前端静态 | HTML、Bootstrap、jQuery、DataTables |
| [polln](#8-polln) | 全栈 | Python、Django、MySQL |
| [seat-booking-app](#9-seat-booking-app) | 纯前端静态（可选 Sass） | HTML / JS；可选 `sass` |
| [taskify](#10-taskify) | 后端渲染 | Node、Express、EJS |

---

## 通用说明：终端与路径

- 下文 **`<REPO>`** 表示本目录的绝对或相对路径，例如：  
  `CPT304_Coursework/third_party/open_source_projects`
- **macOS / Linux**：终端一般为 `bash` 或 `zsh`。  
- **Windows**：建议使用 **PowerShell** 或 **命令提示符（cmd）**；路径用反斜杠或带引号的完整路径。

### Python 虚拟环境（polln 会用到）

| 操作 | macOS / Linux | Windows (PowerShell) | Windows (cmd) |
|------|----------------|----------------------|-----------------|
| 创建虚拟环境 | `python3 -m venv env` | `python -m venv env` | `python -m venv env` |
| 激活 | `source env/bin/activate` | `.\env\Scripts\Activate.ps1` | `env\Scripts\activate.bat` |
| 退出虚拟环境 | `deactivate` | `deactivate` | `deactivate` |

### 用浏览器打开本地 HTML（静态项目）

- **macOS**：在 Finder 中双击 `index.html`，或在终端执行  
  `open index.html`（macOS 专用）。
- **Windows**：在资源管理器中双击 `index.html`，或在 PowerShell 中执行  
  `start index.html`。

静态站点更推荐使用 **本地 HTTP 服务**（见各节「方式二」），可避免部分浏览器对 `file://` 的限制。

### 本地静态服务（Python，适用于多数纯前端项目）

在**项目根目录**（与 `index.html` 同级）执行：

```bash
# macOS / Linux / Windows（需已安装 Python 3）
python3 -m http.server 8080
```

若系统只有 `python` 命令：

```bash
python -m http.server 8080
```

然后在浏览器访问：`http://127.0.0.1:8080/` 或 `http://localhost:8080/`。

**停止服务**：在运行该命令的终端窗口按 **Ctrl+C**。

---

## 1. advanced-finance-tracker

**环境**：无需 Node；可选 Python 仅用于本地 HTTP 服务。

**方式一：直接打开**

进入 `advanced-finance-tracker`，用浏览器打开 `index.html`。

**方式二：本地服务（推荐）**

```bash
cd <REPO>/advanced-finance-tracker
python3 -m http.server 8080
```

访问 `http://127.0.0.1:8080/`。

---

## 2. biztrack

**环境**：同上，纯静态。

```bash
cd <REPO>/biztrack
python3 -m http.server 8080
```

或双击打开 `index.html`。线上演示见原仓库 README。

---

## 3. budget-app

**环境**：纯静态。

```bash
cd <REPO>/budget-app
python3 -m http.server 8080
```

---

## 4. e-commerce

**环境**：纯静态（样式多为仓库内已编译的 CSS；若需自行改 SCSS 再编译，需额外安装 Sass/编译工具，此处从略）。

```bash
cd <REPO>/e-commerce
python3 -m http.server 8080
```

入口为 `index.html`。

---

## 5. employee-management-system

**环境**：**Node.js（建议 LTS）**、**MongoDB**（本机或 Atlas 云数据库）。

### 5.1 安装依赖

**macOS / Linux / Windows（命令相同）**：

```bash
cd <REPO>/employee-management-system/server
npm install

cd ../client
npm install
```

### 5.2 配置数据库

在 **`server`** 目录创建 **`.env`**（可参考同目录 `.env.sample`），至少包含：

```env
DATABASE_URL=mongodb://127.0.0.1:27017
PORT=8000
```

说明：`DATABASE_URL` 不要带库名后缀，项目会连接名为 `EMS` 的数据库。若使用 **MongoDB Atlas**，将连接串填到 `DATABASE_URL`。

### 5.3 启动 MongoDB

- **macOS（Homebrew）**：`brew services start mongodb-community`（包名因安装方式可能略有不同）。
- **Windows**：使用 MongoDB 官方安装包安装后，通过「服务」启动 **MongoDB**，或使用 Atlas 云数据库（无需本机进程）。

### 5.4 启动项目（需两个终端）

**终端 A — 后端：**

```bash
cd <REPO>/employee-management-system/server
npm run dev
```

默认 API：`http://localhost:8000`。

**终端 B — 前端：**

```bash
cd <REPO>/employee-management-system/client
npm run dev
```

浏览器访问终端中提示的地址（一般为 **`http://localhost:5173`**）。

**停止**：在两个终端分别 **Ctrl+C**。

---

## 6. inventory-app-js

**环境**：**Node.js**；仓库内已包含 `public/build` 构建产物时，可仅起静态服务预览。

### 6.1 仅预览（使用已有构建结果）

```bash
cd <REPO>/inventory-app-js/public
python3 -m http.server 8080
```

访问 `http://127.0.0.1:8080/`。

### 6.2 完整开发流程（改源码时）

```bash
cd <REPO>/inventory-app-js
npm install
```

- **监听 Tailwind CSS**：`npm run dev`（会监视并生成 `public/build/style.css`）。
- **修改 `src/js` 后**需按 `package.json` 中的脚本执行 Babel 与 Webpack，例如：

```bash
npm run app-babel
npm run categoryView-babel
npm run productView-babel
npm run storage-babel
npm run build
```

一次性压缩样式：`npm run tailwind-minify`。

---

## 7. inventory-management-system

**环境**：纯静态；`DB.txt` 为示例 JSON 数据，可在页面中加载。

```bash
cd <REPO>/inventory-management-system
python3 -m http.server 8080
```

浏览器打开 `http://127.0.0.1:8080/`，按页面说明使用 **LOAD DATA / NEW DATA / SAVE DATA**。

---

## 8. polln

**环境**：**Python 3**、**MySQL**（本机或 Docker）、pip 依赖见 `requirements.txt`。

### 8.1 虚拟环境与依赖

```bash
cd <REPO>/polln

# macOS / Linux
python3 -m venv env
source env/bin/activate

# Windows (PowerShell)
# python -m venv env
# .\env\Scripts\Activate.ps1

pip install -r requirements.txt
```

### 8.2 MySQL 与编译依赖说明

- Django 使用 **`django.db.backends.mysql`**，需 Python 包 **`mysqlclient`**。
- **macOS**：建议先安装 **MySQL** 与 **pkg-config**，再安装依赖，例如：  
  `brew install mysql pkg-config`  
  然后执行 `pip install -r requirements.txt`（若 `mysqlclient` 仍失败，请确认已安装 Xcode Command Line Tools：`xcode-select --install`）。
- **Windows**：安装 [MySQL Community Server](https://dev.mysql.com/downloads/mysql/)，并安装 **Visual C++ Build Tools** 以便编译 `mysqlclient`；或将 `requirements.txt` 中驱动按官方 README 调整为可安装的连接器（见原项目说明）。

### 8.3 环境变量

复制 `polln/.env.example` 为 **`polln/.env`**，填写 **`MYSQL_USER`**、**`MYSQL_PASSWORD`**，并与本机 MySQL 账户一致。示例库名常为 **`pollnmysql`**（以 `.env.example` 为准）。

### 8.4 创建数据库与迁移

```bash
cd <REPO>/polln
# 确保已激活 venv 且 MySQL 已启动

python mydb.py
python manage.py makemigrations website
python manage.py makemigrations dashboard
python manage.py migrate
```

可选创建管理员：`python manage.py createsuperuser`

### 8.5 启动

```bash
python manage.py runserver
```

浏览器访问 **`http://127.0.0.1:8000/`**。

**停止**：终端 **Ctrl+C**。

---

## 9. seat-booking-app

**环境**：可直接用浏览器打开；**可选**使用 Node 安装 `sass`（仅当需要编译 SCSS 时）。

**方式一：静态访问**

```bash
cd <REPO>/seat-booking-app
python3 -m http.server 8080
```

**方式二：安装依赖（可选）**

```bash
cd <REPO>/seat-booking-app
npm install
```

当前 `package.json` 无 `start` 脚本；若无编译需求，不必执行 `npm install`。

---

## 10. taskify

**环境**：**Node.js**。

### 10.1 安装与配置

```bash
cd <REPO>/taskify
npm install
```

在项目根目录创建 **`.env`**，**务必设置端口**（默认代码会尝试使用 **80**，在 macOS/Windows 上绑定 80 常需管理员权限）：

```env
PORT=3000
```

### 10.2 启动

```bash
npm start
```

浏览器访问 **`http://127.0.0.1:3000/`**。

开发时自动重启可使用：

```bash
npm run server
```

（使用 `nodemon`，依赖已在 `package.json` 的 `devDependencies` 中。）

**停止**：**Ctrl+C**。

**说明**：`src/db/conn.js` 若为空，则 MongoDB 未接入，项目仍可启动并访问页面路由；完整数据库功能需自行补全连接代码。

---

## 常见问题（简要）

| 现象 | 可能原因 |
|------|----------|
| 静态页脚本异常 | 尝试用 `python -m http.server` 代替直接双击 `index.html`。 |
| `pip install mysqlclient` 失败 | macOS：安装 `pkg-config` 与 MySQL 开发文件；Windows：安装 VC++ 构建工具与 MySQL。 |
| `Permission denied` 绑定端口 | 换用高位端口（如 3000、8080），或避免使用 80/443。 |
| PowerShell 无法运行脚本 | 以管理员执行：`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`（按需）。 |

---

## 端口占用参考

| 项目 | 默认或常用端口 |
|------|----------------|
| employee-management-system（后端） | 8000 |
| employee-management-system（Vite） | 5173 |
| polln（Django） | 8000 |
| taskify（需在 `.env` 设置） | 建议 3000 |
| Python `http.server` | 示例中使用 8080 |

若端口被占用，可更换为其他未占用端口，或结束占用进程后再启动。

---

*文档随仓库结构整理；若上游项目更新运行方式，请以各子目录内 `README.md` 为准。*
