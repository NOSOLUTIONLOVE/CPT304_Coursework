# 步骤 2：四个缺陷（按严重程度从高到低）

**项目**：Inventory Management System (UNIQLO)  
**范围**：`projects/inventory-management-system/app/`（以 `index.html` 内联脚本为主）  
**目标**：本文件仅收录用于 **步骤 2** 的 **4 个“源码中已存在”的缺陷**，并给出可复现的现象、根因、影响、修复方向，以及步骤 3 文献检索关键词。

> 说明：本文件聚焦“缺陷（defect/bug）”而非“未实现特性”。每一项都应能独立复现、独立定位到源码责任点。

---

## 缺陷清单（Top 4）

- **P1-1**：删除确认按钮事件重复绑定 → 可能“一次点击多次删除/误删多行”
- **P1-2**：LOAD DATA 解析逻辑脆弱（字符串切割 + `JSON.parse`）→ 格式稍变即崩溃/无提示
- **P1-3**：表单提交使用全局 `event.preventDefault()` → 部分环境直接 `event is not defined`，阻断新增/更新
- **P2-1**：Add/Edit 两个表单复用相同 input `id` → HTML 规范违规、选择器歧义、a11y/维护风险

严重度排序原则：优先选择会导致 **数据误操作**、**核心流程阻断**、**崩溃/不可用** 的问题，其次是会引入高概率隐性 bug 与质量风险的问题。

---

## P1-1 删除确认按钮事件重复绑定（高危：误删/重复执行）

### 现象（可观察）
用户多次点击表格某行的“删除”图标（垃圾桶），即使中途不点 Delete（关闭对话框），重复几次后，再点击一次 Delete，可能出现：

- **一次 Delete 导致删除多行**（或删除行为被执行多次）
- 表格状态/提示与预期不一致（多次 `draw()` 造成闪动或误判）

### 复现步骤（建议写进报告）
1. 打开 `app/index.html`（任意有数据的场景，使用 `LOAD DATA` 导入 `DB.txt` 更方便观察）。  
2. 在同一行或不同多行上重复：点击垃圾桶 → 弹出 Delete Confirmation → 点击右上角关闭（不删除）。重复 3 次以上。  
3. 再次点击某行垃圾桶 → 这次点击 Delete。  
4. 观察：是否发生“删除不止一行/删除动作触发多次”的异常。

### 根因定位（源码证据）
问题本质是 **事件处理器在一个会被反复触发的回调中反复绑定**，且没有解绑或“一次性绑定”。

```110:115:projects/inventory-management-system/app/index.html
$('#myTable').on('click', 'td.editor-delete', function () {
  a = this;
  $('#deleteModal').modal('toggle');
  $('#deleteModal').on('click', '#delete_button', function () {
    delete_Row(a);
  });
});
```

每次点垃圾桶都会执行一次 `$('#deleteModal').on('click', '#delete_button', ...)`，导致 handler **累积叠加**。当用户最终点 Delete 时，会触发多个 handler。

### 影响评估
- **数据完整性风险**：误删多条库存记录，且前端无撤销机制。
- **用户信任与可用性**：删除是破坏性操作，任何“重复执行”都属于高危缺陷。
- **维护风险**：问题在交互次数多时才出现，容易逃过简单演示，但在真实使用会被频繁触发。

### 修复方向（报告中可简述）
推荐策略之一（任选其一）：
- **绑定一次** Delete 按钮事件，在点击垃圾桶时只更新“当前待删除行”的引用；
- 或使用 `.one(...)` / 在绑定前 `.off(...)` 防止累积绑定；
- 也可用事件委托绑定到更稳定的容器，并显式管理 modal 生命周期。

### 文献检索关键词（步骤 3）
- **核心主题**：重复绑定事件 / 事件监听器累积 / UI 事件处理缺陷
- **关键词（英文）**：
  - `jQuery event handler multiple binding`
  - `event listener accumulation bug`
  - `duplicate click handler modal dialog`
  - `memory leak event listeners single page application`
  - `idempotent event binding pattern`
- **关键词（中文，可选）**：
  - `事件重复绑定 缺陷`
  - `模态框 删除按钮 重复触发`

---

## P1-2 LOAD DATA 解析逻辑脆弱（高危：核心流程易崩溃/无提示）

### 现象（可观察）
当用户加载的文件稍微偏离预期格式（例如字段顺序变化、导出结构变化、内容中出现额外的 `}` 片段、或文件不是 DataTables 导出格式）时：

- 点击 `LOAD DATA` 选择文件后，**数据不显示**或脚本报错导致页面逻辑中断
- 缺少清晰的错误提示（用户不知道哪里错了）

### 复现步骤（建议写进报告）
1. 打开 `app/index.html`。  
2. 准备一个“几乎正确但不完全符合当前切割假设”的 JSON 文件，例如：
   - 把 `DB.txt` 外层结构轻微改动（多加一个字段或调整结构），或
   - 使用不同来源导出的 JSON（字段名相同但结构不同）。  
3. 点击 `LOAD DATA` 选择该文件。  
4. 观察：控制台报错/页面无响应/表格未更新且无用户提示。

### 根因定位（源码证据）
导入逻辑不是按 JSON 结构解析，而是先用字符串切割从 `contents` 中“截”出一段再 `JSON.parse`：

```242:246:projects/inventory-management-system/app/index.html
function displayContents(contents) {
  dataSrc = JSON.parse(contents.split('"body":')[1].split('}')[0]);
  $('#myTable').dataTable().fnClearTable();
  $('#myTable').dataTable().fnAddData(dataSrc);
}
```

关键问题：
- `contents.split('"body":')[1]` 假设文件必含 `"body":` 且只出现一次；
- `.split('}')[0]` 假设第一个 `}` 就是数组结束位置（这在一般 JSON 中并不成立）；
- `JSON.parse` 没有 `try/catch`，一旦失败会抛异常并中断后续逻辑；
- 没有错误 UI 提示与回退策略。

### 影响评估
- **核心功能阻断**：应用的主要工作流之一就是导入已有库存数据；解析脆弱等于把主流程建立在不可靠假设上。
- **数据可靠性**：即使不崩溃，也可能解析出错误片段，导致加载数据缺失/错位。
- **可扩展性差**：一旦更换导出格式、升级 DataTables、或加入新字段，导入立刻失效。

### 修复方向（报告中可简述）
更可靠的策略：
- 直接 `const obj = JSON.parse(contents)`；
- 通过显式字段访问拿到数据（例如 `obj.body`），并对结构做校验（数组、列数等）；
- 失败时 `try/catch` 给出错误提示，并保持当前表格状态不被破坏。

### 文献检索关键词（步骤 3）
- **核心主题**：健壮性解析 / 防御式编程 / 输入验证 / 容错导入
- **关键词（英文）**：
  - `robust JSON parsing defensive programming`
  - `input validation file import JSON schema`
  - `error handling JSON.parse try catch UI`
  - `string parsing anti-pattern structured data`
  - `fault tolerance data import pipeline`
- **关键词（中文，可选）**：
  - `JSON 解析 健壮性`
  - `防御式编程 输入校验`

---

## P1-3 表单提交使用全局 event（高危：新增/更新在部分环境不可用）

### 现象（可观察）
在某些浏览器/运行模式下（尤其是更严格的环境、或未来把脚本改为模块化打包后），点击 Add 或 Update 提交表单时可能直接报错：

- `ReferenceError: event is not defined`
- 结果是：**新增/更新完全失效**（核心 CRUD 中的 C/U 被阻断）

### 复现步骤（建议写进报告）
1. 打开 `app/index.html`。  
2. 点击 `ADD NEW ITEM`，随意填写后点击 Add（或编辑后点击 Update）。  
3. 在更严格的执行环境/某些浏览器设置下，观察控制台是否出现 `event is not defined`，以及提交是否失败。

> 注：不同浏览器对“全局 event”是否存在并不一致；因此该缺陷属于典型的**兼容性/健壮性 bug**：在你的环境可能“看似能用”，但从源码层面它是不可靠实现。

### 根因定位（源码证据）
回调没有接收事件对象，却调用了 `event.preventDefault()`：

```128:142:projects/inventory-management-system/app/index.html
$('#edit_data').on('submit', function () {
  event.preventDefault();
  // ...
});

$('#add_data').on('submit', function () {
  event.preventDefault();
  // ...
});
```

标准写法应为 `function (e) { e.preventDefault(); ... }`。当前实现依赖一个“可能存在的全局变量”，属于不可移植/不规范用法。

### 影响评估
- **功能不可用风险**：一旦触发，直接破坏新增/更新功能。
- **未来演进风险**：任何引入严格模式、构建工具、或迁移到模块化代码的尝试都可能让该缺陷变为“必现崩溃”。

### 修复方向（报告中可简述）
把匿名函数签名改为显式接收事件对象：
- `$('#edit_data').on('submit', function (e) { e.preventDefault(); ... })`
- `$('#add_data').on('submit', function (e) { e.preventDefault(); ... })`

### 文献检索关键词（步骤 3）
- **核心主题**：浏览器兼容性 / JavaScript 事件对象 / 非标准全局变量依赖
- **关键词（英文）**：
  - `JavaScript global event is undefined`
  - `cross browser event object best practice`
  - `strict mode event undefined ReferenceError`
  - `jQuery submit handler preventDefault event parameter`
- **关键词（中文，可选）**：
  - `浏览器兼容 event 未定义`
  - `JavaScript 事件对象 最佳实践`

---

## P2-1 表单字段重复 id（中危：规范违规 + 歧义选择器 + a11y/维护风险）

### 现象（可观察）
当前页面里同时存在两个 modal（Edit 与 Add），它们内部的输入框使用了相同 `id`（如 `itemname`、`quantity` 等）。这会导致：

- DOM 层面 `id` 唯一性被破坏（HTML 规范违规）
- 若未来有代码用 `getElementById('itemname')` 或 `#itemname` 直接选择，**会选到“第一个”**，造成错填/错读
- `label for="..."` 的关联可能出现歧义，影响可访问性与自动化测试稳定性

### 复现步骤（建议写进报告）
1. 打开 `app/index.html`。  
2. 同时打开 Add modal 与 Edit modal（或依次打开观察 DOM）。  
3. 在控制台执行 `document.querySelectorAll('#itemname').length`（或同类 id），将发现数量 > 1。  
4. 进一步尝试 `document.getElementById('itemname')`，验证它只返回其中一个元素，说明潜在歧义。

### 根因定位（源码证据）
Add 与 Edit 两个表单都包含相同 `id`：

```352:409:projects/inventory-management-system/app/index.html
<!-- Edit modal -->
<input ... id="itemname" ...>
<input ... id="quantity" ...>
<input ... id="type" ...>
<input ... id="unit_price" ...>
<input ... id="total_price" ...>

<!-- Add modal -->
<input ... id="itemname" ...>
<input ... id="quantity" ...>
<input ... id="type" ...>
<input ... id="unit_price" ...>
<input ... id="total_price" ...>
```

### 影响评估
- **隐性 bug 温床**：当前代码靠 `$('#edit_data').find('input[id="..."]')` 规避一部分歧义，但这是一种“侥幸正确”；任何后续改动都可能让问题显性化。
- **测试与可访问性成本上升**：重复 id 会让端到端测试选择器更脆弱，也会影响辅助技术对表单关联的理解。

### 修复方向（报告中可简述）
推荐：
- 为 Add/Edit 分别使用不同 id（例如 `add_itemname` / `edit_itemname`），或
- 使用 `name` 属性表达字段语义，并通过“表单作用域 + name”选择器取值。

### 文献检索关键词（步骤 3）
- **核心主题**：HTML 规范 / DOM 唯一性约束 / 表单可访问性 / 可测试性
- **关键词（英文）**：
  - `duplicate id HTML accessibility`
  - `HTML id must be unique form label for`
  - `DOM getElementById first match bug`
  - `test automation brittle selectors duplicate id`
- **关键词（中文，可选）**：
  - `HTML 重复 id 影响`
  - `label for 重复 id 可访问性`

