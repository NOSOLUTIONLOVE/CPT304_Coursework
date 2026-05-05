# Inventory 应用源码审查记录

**日期：** 2026-04-27  
**范围：** `projects/inventory-management-system/app/`（主文件 `index.html` 内联脚本；`assets/js/theme.js`、`bs-init.js`；未逐行审 minified 第三方库）  
**方法：** 人工通读 + 行号定位。本仓库未执行 `ce-code-review` 多代理流水线；结论用于课程**步骤 2 候选缺陷**，正式报告前须在本地复现并独立表述。

**相关说明：** 步骤 2 的「四缺陷」须为**源码中已存在**的问题；**勿将步骤 4 的基准缺失**（全站双语、Cookie 横幅等）单独充作四缺陷，除非能论证为错误实现。见 `defect-identification.md`。

---

## 1. 源码级缺陷常见类型（便于分类）

| 类别 | 含义 | 适用示例 |
|------|------|----------|
| 功能 / 业务逻辑 | 与预期行为不一致 | 未删除、计算错误、保存错格式 |
| 数据校验与完整性 | 非法值、NaN、导入不一致 | 空输入进表、解析失败无提示 |
| 交互与状态 | 事件重复绑定、模态与回调 | 同按钮多次触发、未解绑 |
| 无障碍 (a11y) | 语义、名称、landmark | 无 `lang`、图标无可访名称、缺 `main` |
| 安全 | 注入、不安全 DOM、文件 | 需结合是否处理不可信内容 |
| 健壮性 / 错误处理 | 解析、边界 | `JSON.parse` 无 try、格式假设过强 |
| 标准与可维护性 | 无效 HTML、重复 id | 多表单共用同一 `id`、死代码 |

---

## 2. 审查发现（按严重度）

### P1 — 高

| # | 文件与行 | 问题摘要 |
|---|----------|----------|
| 1 | `app/index.html` 约 110–116 | 每次点击删除行打开删除模态时，在 `#deleteModal` 上**再绑定**一次 `#delete_button` 的 `click`；未 `off` 或一次性委托，可能导致**多次删除/重复执行**。 |
| 2 | `app/index.html` 约 128–129、140–141 | 表单子处理器使用**全局** `event.preventDefault()` 且无 `e` 参数；应改为 `function (e) { e.preventDefault(); }`，避免在严格或部分环境下未定义。 |
| 3 | `app/index.html` 约 242–244 | 加载 `DB.txt` 时用 `contents.split('"body":')` 与 `split('}')[0]` 再 `JSON.parse`；**强依赖** DataTables 导出结构，对内容中含 `}`、格式变化**脆弱**；`JSON.parse` 无显式错误处理。 |

### P2 — 中

| # | 文件与行 | 问题摘要 |
|---|----------|----------|
| 4 | `app/index.html` 表单与约 350–409 | Add/Edit 表单中**重复使用相同 `id`**（如 `itemname`、`quantity` 等），违反 HTML id 唯一性；`getElementById` 与部分 a11y 关联易错。 |
| 5 | `app/index.html` 约 130–136、142–147 | `parseFloat` 在非法输入时得 **NaN**，`toFixed` 与行数据可能写出 **"NaN"** 或无效展示；**缺少**统一校验与错误提示。 |
| 6 | `app/index.html` 约 2、284–285 | `<html>` **缺少 `lang`**；**无 `<main>`** 等主 landmark。若作为步骤 2 缺陷，需在报告中**独立论证**为错误/不完备实现，而非仅「未做步骤 4 优化」。 |
| 7 | `app/index.html` 约 38–45、420–460、483 | 操作为**纯图标** `<i>` 时**缺少** `aria-label` 或可见文本；**Logo/空态图** `<img>` **缺少**有意义或明确的 `alt` 策略。 |

### P3 — 低 / 其他

| # | 文件与行 | 问题摘要 |
|---|----------|----------|
| 8 | `app/index.html` 约 264–269 | 定义 `getSubstring` **未使用**（死代码）。 |
| 9 | `app/assets/js/theme.js` 约 44–57 | 若存在 `body.fixed-nav .sidebar`，在**原生**元素上调用 **`.on`**（jQuery API）会失败；**当前单页无该 DOM**，为**潜在死分支/复制残留**；若将来套模板需修正。 |
| 10 | `app/index.html` 约 428–448 | 侧栏多按钮 **`onclick` + `alert` 装饰**；可维护性/产品一致性问题，是否计入「四缺陷」由组内与评分标准定。 |

---

## 3. 结论

- **审阅判断：** 作为教学用单页应用，存在若干**可写进步骤 2** 的逻辑、健壮性、可访问性相关点；**不应**不经验证照抄行号进正式报告。  
- **建议下一步：** 在 `projects/inventory-management-system/docs/templates/defect-log-template.md` 中选定 **4 条**独立缺陷，为每条写清：现象、复现步骤、根因行号、与文献（步骤 3）对应关系。

---

## 4. 上游与协作仓库（仅作文档交叉引用）

- 课程列表上游：见 `README.md` 中「官方课程列表与上游」及 `app/WORKSPACE-SOURCE.md`。  
- 本审查针对 **`inventory-management-system/app/`** 当前快照，与根仓库 `CPT304_Coursework` 的提交以课堂要求为准。
