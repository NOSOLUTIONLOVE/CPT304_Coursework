# 步骤 3：相关文献（按缺陷严重度排序）

用途：把 `../defects/step2-top4-defects.md` 里的四条缺陷，补充对应的“引用/地址（URL/DOI）”。

当前说明：你要求“先填 P1-1”，所以其它缺陷先留空占位，等你后续继续让我补齐。

---

## P1-1：删除确认按钮事件重复绑定（重复处理器累积）

1. **BLeak: Automatically Debugging Memory Leaks in Web Applications** — Vilk, J.; Berger, E. D. (PLDI 2018).  
   DOI/地址：https://dl.acm.org/doi/10.1145/3192366.3192376

2. **Proactive Debugging of Memory Leakage Bugs in Single Page Web Applications** — Shahoor, A.; Abdyldayev, S.; Hong, H.; Yi, J.; Kim, D. (IEEE TSE, 2025).  
   DOI/地址：https://doi.org/10.1109/TSE.2025.3571192

3. **LeakPair: Proactive Repairing of Memory Leaks in Single Page Web Applications** — Shahoor, A.; Yeltayuly Khamit, A.; Yi, J.; Kim, D. (ASE 2023).  
   DOI/地址：https://doi.org/10.1109/ASE56229.2023.00097

4. **Learning How to Listen: Automatically Finding Bug Patterns in Event-Driven JavaScript APIs** — Arteca, E.; Schäfer, M.; Tip, F. (IEEE TSE, 2023，Journal First 对应 FSE 2022）。  
   DOI/地址：https://doi.org/10.1109/TSE.2022.3147975

5. **Static analysis of event-driven Node.js JavaScript applications** — Madsen, M.; Tip, F.; Lhoták, O. (OOPSLA 2015).  
   DOI/地址：https://dl.acm.org/doi/10.1145/2858965.2814272

6. **JSWhiz: Static analysis for JavaScript memory leaks** — Pienaar, J. A.; Hundt, R. (CGO 2013).  
   DOI/地址：https://doi.org/10.1109/CGO.2013.6495007

---

## P1-2：LOAD DATA 字符串切割 + JSON 解析脆弱（输入处理 / 容错）

1. **On-demand JSON: A better way to parse documents?** — Keiser, J.; Lemire, D. (Software: Practice and Experience, 2024).  
   DOI/地址：https://doi.org/10.1002/spe.3313

2. **Validation of Modern JSON Schema: Formalization and Complexity** — Attouche, L.; Baazizi, M.-A.; Colazzo, D.; Ghelli, G.; Sartiani, C.; Scherzinger, S. (Proceedings of the ACM on Programming Languages, POPL 2024).  
   DOI/地址：https://doi.org/10.1145/3632891

3. **Foundations of JSON Schema** — Pezoa, F.; Reutter, J. L.; Suárez, F.; Ugarte, M.; Vrgoč, D. (WWW 2016).  
   DOI/地址：https://doi.org/10.1145/2872427.2883029

4. **JSON: Data model, Query languages and Schema specification** — Bourhis, P.; Reutter, J. L.; Suárez, F.; Vrgoč, D. (PODS 2017).  
   DOI/地址：https://doi.org/10.1145/3034786.3056120

5. **Everything You Always Wanted to Know About JSON Schema (But Were Afraid to Ask)** — Baazizi, M.-A.; Colazzo, D.; Ghelli, G.; Sartiani, C.; Scherzinger, S. (EDBT 2025).  
   DOI/地址：https://doi.org/10.48786/edbt.2025.116

---

## P1-3：表单提交使用全局 `event`（跨浏览器兼容 / 事件对象规范）

1. **DOM Standard (WHATWG):** Event / EventTarget / 事件监听器模型（回调接收 `Event` 对象）。  
   地址：https://dom.spec.whatwg.org/

2. **HTML Standard (WHATWG):** Web app APIs / event handler invocation（事件处理器调用方式）。  
   地址：https://html.spec.whatwg.org/dev/webappapis.html

3. **Window: event property (MDN):** 说明 `window.event` 属于非标准/不推荐用法，应使用事件处理器参数里的 `Event` 对象。  
   地址：https://developer.mozilla.org/en-US/docs/Web/API/Window/event

---

## P2-1：重复 `id`（HTML 唯一性 / a11y / 测试脆弱性）

1. **id（HTML Global Attribute）— must be unique（WHATWG HTML Living Standard）**  
   地址：https://html.spec.whatwg.org/multipage/dom.html#global-attributes-id

2. **F77: Failure of Success Criterion 4.1.1 due to duplicate values of type ID（W3C Techniques for WCAG 2.0）**  
   地址：https://www.w3.org/TR/WCAG20-TECHS/F77.html

3. **Providing Accessible Names and Descriptions (WAI-ARIA APG)**  
   地址：https://www.w3.org/WAI/ARIA/apg/practices/names-and-descriptions/

4. **id attribute value must be unique | Axe Rules**（用于自动化可测性/质量门控）  
   地址：https://dequeuniversity.com/rules/axe/4.6/duplicate-id
