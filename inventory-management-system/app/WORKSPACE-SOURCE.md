# 本目录源码与课程协作说明（CPT304）

## 快照来源

| 字段 | 值 |
|------|-----|
| 复制自 | `Open_Source_Project/inventory-management-system/`（课程打包样本，与官方列表仓库同源） |
| 快照日期 | 2026-04-21 |
| 课程列表上游 | `https://github.com/sptin2002/inventory-management-system` |

## 与小组 fork 的关系

- **日常开发仍以小组 fork 为准**（PR、Issue、部署预览）。  
- **`app/` 本快照** 用于：本仓库内联修改、与 `docs/` 文档同一套提交、课程 zip 打包。  
- 若 fork 与本快照**出现分歧**，以 **fork 上为权威**，并定期将 fork 合并结果同步回 `app/`（或反向，由组内定单一主库）。

## 避免双主库漂移

1. 每次从 fork 同步到本目录时，在本文件或下方「同步记录」中记下 **日期与 commit SHA**。  
2. 或以脚本/rsync 单向从 fork clone 目录同步到 `app/`。

### 同步记录

| 日期 | 方向 | Commit / 说明 |
|------|------|----------------|
| 2026-04-21 | `Open_Source_Project/…` → `app/` | 初始完整迁移 |

## 与只读样本对照

需要对比课程原始打包行为时，仍可打开：

`Open_Source_Project/inventory-management-system/`

两者应一致；若课程侧更新打包，可再次 rsync 覆盖 `app/` 并更新上表。
