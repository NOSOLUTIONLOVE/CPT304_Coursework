# 应用源码（authoritative source）

## 推荐：以小组 GitHub fork 为准

日常开发与合并请求应在 **小组对 `sptin2002/inventory-management-system` 的 fork** 上进行。  
本仓库根目录 `inventory-management-system/README.md` 中应维护 **fork 的 HTTPS/SSH URL**。

## 本目录 `app/` 扮演什么角色

当前 **默认不强制** 在本仓库内再放一份完整源码树，避免与 fork **双主库无说明** 漂移。

可选策略（组内只选其一并写进主 `README.md` 的表格）：

1. **仅链接**：`app/` 只有本 README，源码仅在 fork。提交课程 zip 时再从 fork 打标签或导出。  
2. **定期快照**：每当里程碑结束，将 fork 某 commit 的源码复制到 `app/`，并在 commit message 或本文件脚注中记录 **日期与 commit SHA**。  
3. **Git 子模块**：`app/` 子模块指向 fork（需要全员熟悉 submodule）。

## 课程对照样本

需要对比「原版打包行为」时，使用只读路径：

`Open_Source_Project/inventory-management-system/`

该样本与官方列表仓库应同源；若发现差异，以 **你组 fork 的 `main`** 为准写报告。
