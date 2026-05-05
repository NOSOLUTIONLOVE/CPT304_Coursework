# Deficiency Audit 对比文档：His 分析 vs Ours vs 组员 AI（仓库口径/步骤2口径）

**His 分析来源**：你在聊天中粘贴的 `D1–D9`（如 `Stored XSS`、`Race Condition`、`Data Loss on Refresh`、`Event Listener Leak`、`Google Fonts`、`ARIA` 等）。  
**Ours 分析来源（仓库口径）**：  
- `./step2-top4-defects.md`（步骤 2：只收录"源码中已存在"的四缺陷）  
- `./source-defects-ranked.md`（同一批审查的扩展清单，按严重度归类）

**组员 AI 分析来源（截图内容）**：  
- 缺陷 1：缺少可访问性结构（Lighthouse/DevTools 发现 table headers 使用 `<td>`、缺 skip link、编辑/删除按钮缺少 aria-label 等）  
- 缺陷 2：XSS（声称通过 `innerHTML` 注入/渲染触发）  
- 缺陷 3：缺少客户端输入校验（空/负数/非法值未阻止，且相关 aria/错误提示缺失）  
- 缺陷 4：缺少国际化/语言切换（缺 `lang` 属性、无语言切换，截图里给出 i18n data-attribute 修复方案）

---

## 1. 为什么两份分析的“Top 缺陷集合”会不同

两份分析的**目标口径**不同，因此“是否入选”规则自然不同：

- **His**：把 `D1–D9` 作为“缺陷候选池”，并标注 `Selected/Not selected`，包含安全（XSS）、逻辑竞态、刷新数据丢失、性能、可访问性、UX 反馈等广泛领域。
- **Ours**：遵循课程**步骤 2**要求，只把能被明确定位到“实现错误”的“源码中已存在的独立问题”纳入“四缺陷”。  
  也就是说，**步骤 4 那类“基准缺失/未实现特性”通常不应当算作步骤 2 四缺陷**（除非你能论证为“错误实现”）。

因此两份文档不会对齐到同一组 `D1–D4`。

---

## 2. D1–D9 逐项对比表

| His 缺陷 | His 核心论点（摘要） | Ours 是否覆盖（步骤2四缺陷/扩展清单） | 主要对应项 | 差异点 |
|---|---|---|---|---|
| **D1 Stored XSS** | 通过导入数据把 `<script>` 等注入到表格渲染；DataTables 默认渲染原始 HTML | **未纳入步骤2四缺陷** | — | Ours 的四缺陷集合更偏向“可稳定复现且确定为实现错误”的交互/解析/兼容性点 |
| **D2 Race Condition** | 全局可变变量 `a/b` 被快速点击覆盖，导致对错行执行编辑/删除 | **未纳入** | — | Ours 当前缺陷清单未把 `a/b` 竞态写成独立四缺陷 |
| **D3 Data Loss on Refresh** | DataTables 仅内存态，刷新丢失；缺持久化 | **未纳入** | — | Ours 的步骤2口径通常会把“缺少持久化机制”视作接近未实现特性（除非能论证实现错误） |
| **D4 Event Listener Leak** | 每次打开删除模态都重复绑定 click handler；一次点击触发多次 `delete_Row` | **覆盖（强重合）** | `step2-top4-defects.md` 的 **P1-1 删除确认按钮事件重复绑定** | His 强调“多行被删/重复执行”，Ours 同样强调并给出复现与修复方向 |
| **D5 No Input Validation** | 仅用 `replace` 做粗糙过滤，缺范围/格式/空值校验，可能产生非法数据 | **部分覆盖** | `source-defects-ranked.md` 的 **P2-2 输入校验不足（NaN/数据污染）** | His 的校验缺失更宽泛；Ours 聚焦 `parseFloat` -> `NaN` -> 数据污染/错误 |
| **D6 Excessive Google Fonts** | 过多字体外链导致 render-blocking 与性能损耗 | **未纳入** | — | Ours 当前口径优先选择交互/可靠性/兼容性，性能优化点未进入步骤2四缺陷 |
| **D7 Missing Keyboard & ARIA** | 图标无 `aria-label`/模态缺少 dialog role 与 aria 属性 | **覆盖（在扩展清单中）** | `source-defects-ranked.md` 的 **P2-3 可访问性缺陷** | His 列得更细（role/dialog/aria-labelledby），Ours 以 `lang`、图标命名、alt 策略等为重点 |
| **D8 Missing Operation Feedback** | 无加载/错误/成功反馈，导入失败静默 | **未纳入** | — | Ours 步骤2四缺陷不以“反馈体验缺失”作为独立四缺陷入口 |
| **D9 Non-functional Placeholder Buttons** | sidebar 占位按钮仅 alert，不提供功能 | **部分覆盖（方向一致但优先级不同）** | `source-defects-ranked.md` 的 P3/风险提示类 | His 明确给出 UX 低缺陷；Ours 更倾向质量/一致性问题放在低优先级或按团队口径决定是否入选 |

---

## 3. 组员 AI（缺陷 1-4）与 His/Ours 的对比

| 组员 AI 缺陷 | 组员 AI 的核心论点（截图摘要） | 与 His 的对应关系 | 与 Ours 是否覆盖 | 步骤2口径/合格性提示（需要裁决点） |
|---|---|---|---|---|
| 缺陷 1：Missing accessibility structure | table 缺 caption/列头（`<td>` 非 `<th>`）、按钮缺 `aria-label`、缺 skip link 等 | 最接近 His 的 `D7`（Accessibility/ARIA） | **部分重合**：Ours 的 P2-3 里有“图标按钮无可访问名称、缺 lang、缺 alt”等，但 Ours 四缺陷清单未把“table headers/skip link”写成独立四缺陷 | 可能“可做步骤2”，但需把它写成“现有实现的错误实现”，并提供源码落点/复现；否则容易落入步骤4的可访问性基准叙事 |
| 缺陷 2：XSS via `innerHTML` | 声称 table rows 通过 `innerHTML` 拼接并导致 Stored XSS | **强对应 His `D1 Stored XSS`** | **未被 Ours 选为四缺陷** | 建议复核源码：当前 `app/index.html` 的 DataTables 初始化列是 `columns: [null, null, ...]`，且 `index.html` 中未看到明显 `innerHTML` 拼接渲染（仅有 `buttonShow[i].innerHTML` 用于判断）。若源码证据不足，按步骤2要求需要重取证 |
| 缺陷 3：Absent client-side validation | 添加/编辑弹窗未校验，负数/非法输入会写入，并缺少 aria/错误提示 | **强对应 His `D5`（No Input Validation）** | **重合较高**：Ours 的 P2-2（输入校验不足/NaN/数据污染）与其方向一致 | 可作为步骤2候选，但仍要给“可稳定复现步骤 + 源码根因落点 + 为什么算 bug” |
| 缺陷 4：No internationalisation support | 缺 `lang`、无语言切换；截图给出 data-i18n + localStorage 的完整修复方案 | 与 His `D7` 同属无障碍/ARIA，但不在 His `D1–D9` 的明确命名体系内 | **部分重合**：Ours P2-3 包含 `<html>` 缺 `lang`；但“语言切换/i18n 机制”更像步骤4基准 | 需要按 `docs/defect-identification.md` 裁决：缺 `lang` 可在“错误实现”叙事下算步骤2；但“没有 i18n/没有英文”等通常是步骤4基准，除非证明是实现错误而非后续未做特性 |

---

## 4. 最关键的分歧：哪些类型“被 Ours 排除”

从现有仓库口径看，Ours 更容易**排除**这些类型（除非能证明“错误实现”）：

- **XSS / 注入安全**：即便属于严重问题，Ours 的步骤2四缺陷集合目前没有把它作为“可复现四缺陷”写入。
- **竞态 / 全局变量覆盖**：目前没有在四缺陷里形成独立、可复现、可定位到源码的条目。
- **数据持久化缺失**：更接近“缺少功能/未实现”，通常不进步骤2四缺陷集合。

---

## 5. 你接下来可以怎么用这份对比文档

- 如果你要把 His 的 `D1–D4` 映射到课程步骤2四缺陷：  
  你需要用 Ours 这套口径做“合格/不合格裁决”，并补齐每条的“可复现现象 + 源码证据 + 与步骤4基准是否重叠”的论证。
- 若你要更新仓库缺陷登记模板：  
  建议以 `step2-top4-defects.md` 的 P1-1/P1-2/P1-3/P2-1 为骨架，再把你想保留的 His 条目逐个做“是否仍算源码缺陷”的补证。

- 如果你要把“组员 AI 的缺陷1-4”最终纳入报告：  
  建议逐条对照 `docs/defect-identification.md` 的“步骤2合格/不合格示例”，尤其重点处理：  
  - XSS（缺源码证据时需要重取证）  
  - 国际化/语言切换（缺 lang 可算步骤2，缺英文/缺 i18n 通常是步骤4基准）

