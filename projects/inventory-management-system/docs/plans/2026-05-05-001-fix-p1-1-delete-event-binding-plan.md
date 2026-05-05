---
title: "fix: P1-1 delete button event handler duplicate binding"
type: fix
status: completed
date: 2026-05-05
origin: projects/inventory-management-system/docs/brainstorms/2026-05-05-p1-1-delete-event-handler-binding-fix-requirements.md
---

# fix: P1-1 delete button event handler duplicate binding

## Overview

修复 `app/index.html` 中删除确认按钮的事件处理器重复绑定问题。根因：每次点击垃圾桶图标都重新绑定 `#delete_button` 的 click handler，导致用户反复打开模态后 handler 累积，一次点击触发多次 `delete_Row()`。

## Problem Frame

**缺陷编号**：P1-1（步骤 2）
**影响**：反复打开/关闭删除确认模态后，一次 Delete 点击会执行多次 `delete_Row()`，导致误删多行库存记录。
**根因**：`app/index.html` 第 113-115 行，在 `td.editor-delete` 的点击回调内每次都执行 `$('#deleteModal').on('click', '#delete_button', ...)`，从未解绑。
**文献依据**：LeakPair/Shahoor FP2（注册/卸载对偶）、BLeak（重复视图时资源单调增长）、Madsen et al.（事件调用图的注册站点应落在稳定生命周期边界）。

## Requirements Trace

- R1. `#delete_button` 的 click handler 必须只绑定一次（DataTables 初始化完成后）
- R2. 点击垃圾桶图标时只更新 `currentDeleteRow` 引用变量，不再执行任何 `.on()` 调用
- R3. Delete 按钮 handler 读取 `currentDeleteRow` 执行删除，操作完成后置 `null`
- R4. 保留现有删除确认模态

## Scope Boundaries

- 不修复编辑功能中的 `b` 变量问题（属于 P1-2/P2-1）
- 不修复表单 `id` 重复、LOAD DATA 解析、全局 `event` 变量等问题
- 不拆分 JS 模块，修改在现有 `<script>` 块内完成

## Context & Research

### Relevant Code and Patterns

| 位置 | 内容 |
|------|------|
| `app/index.html` L89-L90 | 全局变量 `var a = null; var b = null;` |
| `app/index.html` L92-L108 | `delete_Row(a)` 函数定义 |
| `app/index.html` L110-L116 | **问题代码**：删除按钮处理器每次在回调内重新绑定 |
| `app/index.html` L118-L126 | 编辑按钮处理器（使用全局 `b`，作为参考模式） |

### Bootstrap Modal 生命周期

模态通过 `$('#deleteModal').modal('toggle')` 打开/关闭。模态的隐藏/显示状态由 Bootstrap 管理，但 **不触发任何自动清理**——handler 一旦绑定就持续有效，直到显式解绑或页面刷新。

## Key Technical Decisions

- **方案 A（绑定一次 + 行引用变量）**：在 DataTables 初始化完成后立即绑定 `#delete_button` handler，后续只更新 `currentDeleteRow` 变量。改动最小、风险最低。
- **新变量 `currentDeleteRow`**：将 `a` 从删除流程中解耦，仅编辑功能继续使用 `a`。新的 `currentDeleteRow` 专用于删除流程。
- **保留确认模态**：用户点 Delete 才真正删除，不改为直接删除。

## Open Questions

### Resolved During Planning

- **E2E 测试需求**：Deferred to implementation — 修复后由实施者决定是否补充 Cypress/Playwright 测试用例。

### Deferred to Implementation

- **报告中的代码片段长度**：由实施者根据步骤 5 报告要求自行决定。

## Implementation Units

- [ ] **Unit 1: 声明 `currentDeleteRow` 变量**

**Goal:** 在全局作用域声明新变量 `currentDeleteRow`，用于替代原 `a` 在删除流程中的作用。

**Requirements:** R3

**Dependencies:** None

**Files:**
- Modify: `projects/inventory-management-system/app/index.html`

**Approach:**
在 DataTables 初始化完成后、`td.editor-delete` 点击绑定之前，添加 `var currentDeleteRow = null;` 声明。保留原有 `var a = null;` 不变（编辑功能仍在使用）。

**Test scenarios:**
- Test expectation: none — 纯变量声明，无行为变化

**Verification:**
- `currentDeleteRow` 在控制台访问时为 `null`

---

- [ ] **Unit 2: 重构 `td.editor-delete` 点击处理器**

**Goal:** 移除 handler 内的 `#delete_button` 绑定逻辑，改为只更新 `currentDeleteRow` 引用。

**Requirements:** R1, R2

**Dependencies:** Unit 1

**Files:**
- Modify: `projects/inventory-management-system/app/index.html`

**Approach:**
将第 110-116 行重构：
- 点击垃圾桶图标时：将 `currentDeleteRow = this`（不再赋值给 `a`）
- 打开模态：`$('#deleteModal').modal('toggle')`
- **删除** `$('#deleteModal').on('click', '#delete_button', ...)` 整行

伪代码示意：

```js
// 重构前（第 110-116 行）：
$('#myTable').on('click', 'td.editor-delete', function () {
    a = this;
    $('#deleteModal').modal('toggle');
    $('#deleteModal').on('click', '#delete_button', function () {  // ← 每次都绑定，BUG
        delete_Row(a);
    });
});

// 重构后：
$('#myTable').on('click', 'td.editor-delete', function () {
    currentDeleteRow = this;
    $('#deleteModal').modal('toggle');
});
```

**Patterns to follow:**
- 参考第 118-126 行编辑按钮处理器——它正确地只更新 `b`，不重新绑定 Edit 模态按钮

**Test scenarios:**
- Happy path: 点击垃圾桶 → 打开模态 → 点 Delete → 删除 1 行 → `currentDeleteRow` 为 `null`
- Edge case: 连续打开/关闭删除模态 10 次 → 点 Delete → 仍然只删除 1 行（核心修复验证）
- Edge case: 打开删除模态后不点 Delete，改点 Add Item → 关闭模态 → 再打开删除模态 → 之前的状态不影响新删除

**Verification:**
- 反复打开/关闭模态后，控制台验证 `$('#delete_button')` 的 handler 数量始终为 1

---

- [ ] **Unit 3: 绑定一次 `#delete_button` click handler**

**Goal:** 在 DataTables 初始化完成后立即绑定 Delete 按钮 handler，读取 `currentDeleteRow` 执行删除。

**Requirements:** R1, R3

**Dependencies:** Unit 1

**Files:**
- Modify: `projects/inventory-management-system/app/index.html`

**Approach:**
在 DataTables 初始化结束（第 87 行 `} );` 之后）、全局变量声明之后、`td.editor-delete` 绑定之前，添加：

```js
$('#deleteModal').on('click', '#delete_button', function () {
    if (currentDeleteRow) {
        delete_Row(currentDeleteRow);
        currentDeleteRow = null;
    }
});
```

**Patterns to follow:**
- 与现有编辑功能模式一致（全局 `b` + Edit 模态按钮 handler 在顶层绑定）

**Test scenarios:**
- Happy path: 点击删除 → 点 Delete → 正确删除对应行
- Edge case: `currentDeleteRow` 为 `null` 时点 Delete（不应发生，除非有竞态） → 无操作，安全容忍
- Integration: 删除后立即点另一行垃圾桶 → `currentDeleteRow` 正确更新 → 删除正确行

**Verification:**
- `$._data($('#delete_button').get(0), 'events').click.length === 1` 返回 `true`

---

- [ ] **Unit 4: 手动复现验证**

**Goal:** 按照步骤 2 报告中的复现步骤验证修复有效。

**Requirements:** Success Criteria

**Dependencies:** Unit 2, Unit 3

**Files:**
- None (manual test)

**Approach:**
按照 `docs/defects/step2-top4-defects.md` 中 P1-1 的复现步骤执行：
1. 导入 `DB.txt` 数据
2. 连续 3+ 次"点垃圾桶 → 关闭模态"
3. 点 Delete
4. 确认仅删除 1 行

**Test scenarios:**
- 测试修复后的行为：连续打开/关闭模态 10 次后 Delete 仅删 1 行

**Verification:**
- 通过手动复现，确认修复有效

## System-Wide Impact

- **Unchanged invariants**: 编辑功能（全局 `b`、Edit 模态）完全不受影响；DataTables 配置不变；其他模态（Add/Edit/Confirmations）不变

## Risks & Dependencies

| Risk | Mitigation |
|------|------------|
| 误删 `a` 全局变量（编辑功能仍在使用） | 不修改 `a`，仅新增 `currentDeleteRow` |
| `delete_Row` 参数名冲突 | `delete_Row` 函数的参数名 `a` 与全局变量同名但为局部参数，无需改动 |
| 模态内 Delete 按钮在 DOM 中被重新创建 | 当前模态 DOM 结构固定，若未来变化需重新验证 |

## Documentation / Operational Notes

- 步骤 5 报告 Implementation 段落需引用修改后的代码片段（行号会变化）
- 步骤 3 文献 Implementation 可引用 LeakPair FP2 作为修复依据

## Sources & References

- **Origin document:** [projects/inventory-management-system/docs/brainstorms/2026-05-05-p1-1-delete-event-handler-binding-fix-requirements.md](projects/inventory-management-system/docs/brainstorms/2026-05-05-p1-1-delete-event-handler-binding-fix-requirements.md)
- **Defect doc:** [projects/inventory-management-system/docs/defects/step2-top4-defects.md](projects/inventory-management-system/docs/defects/step2-top4-defects.md) — P1-1
- **Literature remediation:** [projects/inventory-management-system/docs/literature/p1-1-remediation-from-survey.md](projects/inventory-management-system/docs/literature/p1-1-remediation-from-survey.md)
