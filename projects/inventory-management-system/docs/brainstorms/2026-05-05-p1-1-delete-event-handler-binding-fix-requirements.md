---
date: 2026-05-05
topic: p1-1-delete-event-handler-binding-fix
---

# P1-1 删除确认按钮事件重复绑定修复

## Problem Frame

**缺陷编号**：P1-1（步骤 2）
**影响**：用户反复打开/关闭删除确认模态后，Delete 按钮累积多个 click handler，一次点击会触发多次 `delete_Row()`，导致误删多行库存记录。
**根因**：`app/index.html` 第 110-115 行，每次点击垃圾桶图标都执行 `$('#deleteModal').on('click', '#delete_button', ...)`，从未解绑，导致 handler 栈叠加。
**文献支撑**：LeakPair/Shahoor FP2（注册/卸载对偶）、BLeak（重复打开视图时资源单调增长）、Madsen et al.（事件调用图的注册站点应落在稳定生命周期边界）。

---

## Requirements

**事件绑定（修复 P1-1）**
- R1. `#delete_button` 的 click handler 必须在 DataTables 初始化完成后**只绑定一次**，不得在任何会被反复触发的回调（如 `td.editor-delete` 的点击处理器）内再次绑定。
- R2. 点击垃圾桶图标时，只更新「当前待删除行」的引用变量 `currentDeleteRow`，然后打开模态。**不再执行任何 `.on()` 调用**。
- R3. Delete 按钮的 handler 读取 `currentDeleteRow` 执行删除，操作完成后将 `currentDeleteRow` 置 `null`。
- R4. 保留现有的删除确认模态（`#deleteModal`），用户点 Delete 才真正删除。

**行为一致性（不引入新缺陷）**
- R5. 全局变量 `a`（原用于编辑功能）保持不变，仅将 `a` 在删除流程中的作用迁移到新的 `currentDeleteRow`。
- R6. DataTables 的 `draw()` 调用次数不变，不引入新的渲染性能问题。

---

## Success Criteria

- [ ] 连续打开/关闭删除模态 10 次后，点 Delete 只会删除 1 行
- [ ] 控制台无 handler 累积警告（可用 `$._data($0).events` 验证）
- [ ] 原有的编辑功能（`b` 变量、Edit Modal）不受影响
- [ ] 步骤 3 报告可引用 LeakPair FP2 作为修复文献依据

---

## Scope Boundaries

- **不在本修复范围内**：编辑功能中的 `b` 变量问题（P1-2/P2-1 讨论）；表单 `id` 重复问题；LOAD DATA 解析脆弱性（P1-2）；全局 `event` 变量问题（P1-3）
- **不引入新文件**：修复在 `index.html` 的现有 `<script>` 块内完成，不拆分 JS 模块

---

## Key Decisions

- **采用方案 A（绑定一次 + 行引用变量）**：在初始化时绑定 Delete handler，后续只更新 `currentDeleteRow` 变量。改动最小、风险最低，且与课程报告的「修复方向 A」一致。
- **保留确认模态**：用户点击 Delete 才真正删除，不改为直接删除，保证操作的可逆性和用户体验。
- **不用 `.off()` + `.on()`**：当前场景下引入命名空间会增加复杂度，方案 A 从根本上杜绝了累积，无需显式解绑。
- **不用事件委托**：委托到更远的父容器会增加 `event.target` 判断复杂度，方案 A 对现有代码结构改动最少。

---

## Dependencies / Assumptions

- 依赖 jQuery（已有）和 Bootstrap 5 Modal（已有）
- 假设 DataTables 初始化后 `#delete_button` DOM 元素始终存在（用户交互中不会被重新创建）
- 修复后需要手动测试复现步骤，或编写 Cypress/Playwright E2E 测试用例

---

## Outstanding Questions

### Resolve Before Planning
- [none]

### Deferred to Planning
- [技术] 修复后是否需要补充 E2E 测试用例（Cypress）来覆盖「重复打开模态」的边界条件？
- [技术] 步骤 5 报告中「Detection → Literature → Implementation」的Implementation 段落应包含多少行修改后的代码片段？

---

## Next Steps

- ✅ `/ce:plan` — completed
- ✅ `/ce:work` — completed

> 实施记录：`docs/solutions/p1-1-fix-log.md`
