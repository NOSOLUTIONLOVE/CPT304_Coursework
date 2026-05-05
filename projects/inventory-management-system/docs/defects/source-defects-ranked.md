# Inventory Management System 源码缺陷审查（按严重度排序）

**审查范围：** `projects/inventory-management-system/app/`（重点：`index.html` 内联脚本、`assets/js/theme.js`、`assets/js/bs-init.js`）  
**不在范围：** 第三方库 minified 源码（如 `jquery.min.js`、`datatables.min.js` 等）逐行审查，仅在“风险提示”中提及。

---

## 严重度分级（本报告口径）

- **P1（高）**：可导致数据误删/重复执行、应用崩溃、明显安全风险，或稳定阻断核心流程（加载/编辑/保存）。
- **P2（中）**：会导致错误数据、不可预期行为、a11y 明显缺陷或后续演进高风险，但通常有绕过方式。
- **P3（低）**：可维护性/一致性/死代码等问题，不一定立刻影响用户，但会放大未来改动成本。

---

## P1 — 高危缺陷

### P1-1：删除确认按钮事件重复绑定，可能造成“一次点击多次删除”

- **位置（证据）**：`app/index.html`  
  - 绑定入口：第 110 行开始（点击垃圾桶触发）  
  - 在每次打开删除模态时，都执行一次 `$('#deleteModal').on('click', '#delete_button', ...)`（第 113 行）

```110:115:projects/inventory-management-system/app/index.html
			$('#myTable').on('click', 'td.editor-delete', function () {
				a = this;
				$('#deleteModal').modal('toggle');
				$('#deleteModal').on('click', '#delete_button', function () {
					delete_Row(a);
				} );
			} );
```

- **影响**：用户反复打开删除模态后，`#delete_button` 的 click handler 会叠加；一次点击可能触发多次 `delete_Row(a)`，导致**多行被删除**或状态异常（取决于当时 `a` 的引用与 DataTables 行指针）。
- **复现要点**：连续 3 次“点垃圾桶→不删/关闭→再点垃圾桶→…”后，再点一次 Delete，观察是否触发多次删除/多次 `draw()`。
- **修复建议**：
  - 使用 **一次性事件**（例如 `.one(...)`）或在绑定前 `.off(...)`；更推荐把 `#delete_button` 的 handler **绑定一次**，并通过共享状态记录当前待删行。

---

### P1-2：表单提交处理使用全局 `event`，在部分环境下会直接报错

- **位置（证据）**：`app/index.html` 第 128–141 行附近

```128:142:projects/inventory-management-system/app/index.html
			$('#edit_data').on('submit', function () {
				event.preventDefault();
				var final_quantity = parseFloat($('#edit_data').find('input[id="quantity"]').val().replace(/[^0-9.]/g,''));
				$('#edit_data').find('input[id="quantity"]').val(final_quantity.toFixed(0));
				// ...
			} );

			$('#add_data').on('submit', function () {
				event.preventDefault();
				var final_quantity = parseFloat($('#add_data').find('input[id="quantity"]').val().replace(/[^0-9.]/g,''));
				// ...
			} );
```

- **影响**：`event` 不是标准保证存在的全局变量；在严格模式/不同浏览器策略/模块化环境下，可能出现 `ReferenceError: event is not defined`，导致**无法新增/更新**。
- **修复建议**：把回调签名改为 `function (e) { e.preventDefault(); ... }`（或使用 `submit` 事件对象）。

---

### P1-3：LOAD DATA 的解析逻辑依赖字符串切割，极易因格式变化或异常输入崩溃

- **位置（证据）**：`app/index.html` 第 242–245 行附近

```242:246:projects/inventory-management-system/app/index.html
			function displayContents(contents) {
			  dataSrc = JSON.parse(contents.split('"body":')[1].split('}')[0]);
			  $('#myTable').dataTable().fnClearTable();
			  $('#myTable').dataTable().fnAddData(dataSrc);
```

- **影响**：
  - 强依赖导入文件必须包含 `"body":` 且后续 `split('}')` 截断必须恰好落在数组结束；文件稍有变化就会 `TypeError` 或 `JSON.parse` 抛错。
  - 缺少 `try/catch` 与用户提示，表现为**加载失败但无清晰错误信息**，甚至脚本中断。
- **复现要点**：选择一个不完全符合预期结构的 JSON（或手动在 `DB.txt` 中加入额外对象字段/嵌套 `}`）后 LOAD DATA。
- **修复建议**：
  - 直接 `JSON.parse(contents)`，并用 schema/字段检查获取 `body`；失败时给出错误提示并保持现有表格状态不变。

---

## P2 — 中等严重缺陷

### P2-1：Add/Edit 两个表单复用相同 input `id`，违反 HTML 唯一性并埋下选择器歧义

- **位置（证据）**：`app/index.html` 中 Edit 与 Add 两个 modal 都存在同名 id（`itemname/quantity/type/unit_price/total_price`）

```352:409:projects/inventory-management-system/app/index.html
									<label for="itemname" class="mb-1">Item Name</label>
									<input type="text" class="form-control" id="itemname" placeholder="Please enter a value." required ...>
									<!-- ... -->
									<label for="itemname" class="mb-1">Item Name</label>
									<input type="text" class="form-control" id="itemname" placeholder="Please enter a value." required ...>
```

- **影响**：
  - 任何使用 `document.getElementById('itemname')` 的逻辑都将变得不可靠（只会返回第一个）。
  - `label for="..."` 的关联与可访问性树也可能出现不可预期行为。
  - 当前代码大量依赖 `$('#edit_data').find('input[id="..."]')` 规避了部分问题，但这是脆弱约定：未来改动很容易引入真实 bug。
- **修复建议**：给两个表单使用不同 id（如 `edit_itemname` / `add_itemname`），并统一用 `name` + 表单作用域选择器。

---

### P2-2：输入校验不足，非法输入可能产生 `NaN` 并写入表格数据

- **位置（证据）**：`app/index.html` 第 130–147 行附近多处 `parseFloat(...).toFixed(...)`

```129:147:projects/inventory-management-system/app/index.html
				var final_quantity = parseFloat($('#edit_data').find('input[id="quantity"]').val().replace(/[^0-9.]/g,''));
				$('#edit_data').find('input[id="quantity"]').val(final_quantity.toFixed(0));
				var final_unit = parseFloat($('#edit_data').find('input[id="unit_price"]').val().replace(/[^0-9.]/g,''));
				$('#edit_data').find('input[id="unit_price"]').val('RM '+final_unit.toFixed(2));
				var newData = [ ... ];
				datatable.row(b).data(newData).draw();
```

- **影响**：
  - 空字符串、只有 `.`、或超大数等输入会导致 `parseFloat` 返回 `NaN`；`toFixed` 将抛错或产生 `"NaN"` 相关输出，造成**数据污染**或脚本错误。
  - 当前 UI 没有明确的错误提示/阻止提交策略。
- **修复建议**：在写回 UI 与 DataTable 前做数值校验（`Number.isFinite`）、范围限制、以及错误反馈（例如在 modal 内显示校验信息并阻止提交）。

---

### P2-3：可访问性缺陷（缺少 `lang`、图标按钮无可访问名称、图片无 `alt`）

- **位置（证据）**：
  - `<html>` 缺少 `lang`：`app/index.html` 第 2 行
  - 行内操作按钮为纯图标：第 38 / 44 行 `defaultContent: '<i ... />'`
  - 主要图片未提供 `alt`：第 460 / 483 行

```2:2:projects/inventory-management-system/app/index.html
<html>
```

```38:45:projects/inventory-management-system/app/index.html
						defaultContent: '<i class="fas fa-pencil-alt" style="cursor: pointer;"/>',
						orderable: false
					},
					{
						data: null,
						className: "dt-center editor-delete",
						defaultContent: '<i class="fas fa-trash-alt" style="cursor: pointer;"/>',
						orderable: false
					}
```

```460:483:projects/inventory-management-system/app/index.html
											<img src="assets/images/uniqlo_logo.png" style="height: 4.7vh;">
										</div>
									</div>
									<!-- ... -->
									<div class="row" style="margin: auto;" id="empty_animation">
										<img src="assets/images/empty.gif" style="height: 50vh;">
									</div>
```

- **影响**：屏幕阅读器用户难以理解页面语言与关键操作；纯图标操作缺少可访问名称将导致可用性明显下降。
- **修复建议**：补 `lang`；为操作按钮提供可访问名称（例如把图标放入 `<button aria-label="Edit">...</button>`）；为图片添加合适 `alt`（装饰图用空 `alt=""`）。

> 注：若用于课程“步骤 2 的四缺陷”，需要在报告里把它表述成**现有实现的错误/缺陷**（而不是“步骤 4 才要求的新增特性”），并给出清晰复现与影响叙事。

---

## P3 — 低优先级/可维护性缺陷

### P3-1：`assets/js/theme.js` 在原生元素上调用 `.on(...)`（jQuery API），存在潜在运行时错误

- **位置（证据）**：`app/assets/js/theme.js` 第 44–55 行附近

```44:56:projects/inventory-management-system/app/assets/js/theme.js
  var fixedNaigation = document.querySelector('body.fixed-nav .sidebar');
  
  if (fixedNaigation) {
    fixedNaigation.on('mousewheel DOMMouseScroll wheel', function(e) {
      var vw = Math.max(document.documentElement.clientWidth || 0, window.innerWidth || 0);
      // ...
      e.preventDefault();
    });
  }
```

- **影响**：`fixedNaigation` 是 DOM 元素，默认没有 `.on` 方法；如果未来页面加入 `body.fixed-nav .sidebar`，这段代码会直接抛错并中断后续脚本。
- **修复建议**：改用 `addEventListener`，或用 jQuery 包裹 `$(fixedNaigation).on(...)`（并确保 jQuery 存在）。

---

## 风险提示（不计入单一缺陷，但值得注意）

- **依赖与供应链风险**：项目使用 `jQuery`、`DataTables`、`Bootstrap` 等前端库；若升级/替换版本，当前大量“字符串匹配按钮 innerHTML”“脆弱解析”等实现会更容易失效。

