---
name: search-after-rename
description: 项目重命名后需要全局搜索旧名称，确保所有引用都更新
metadata:
  category: methodology
  status: unverified
  created: "2026-04-11"
  verified_count: 0
---

# 工具优先级：项目重命名后全面搜索

## 优先级排序

### 1. **grep 全局搜索旧名称** — 第一步必须
```bash
# 搜索所有包含旧名称的文件
grep -r "旧名称" --include="*.go" --include="*.md" --include="*.yaml" --include="*.json" .
grep -r "旧名称" -l .  # 只列出文件名
```

### 2. **grep 搜索配置文件中的路径引用**
```bash
# 检查 loader.go 等配置
grep -r "旧名称" *.go
# 检查文档
grep -r "旧名称" docs/ README.md
```

### 3. **更新所有找到的引用**
- Go 代码中的 searchPaths、配置路径
- Markdown 文档中的链接和引用
- 配置文件中的名称

### 4. **验证更新完整性**
```bash
# 搜索确认没有遗漏
grep -r "旧名称" . --include="*.go" --include="*.md"
grep -r "新名称" . --include="*.go" --include="*.md"
```

## 典型场景

### 场景：项目重命名（如 ui-verify-ai → ui-check）

**必须检查的位置：**

1. **Go 源代码**
   - `loader.go` 中的 searchPaths 配置
   - 其他 Go 文件中的包名引用

2. **文档文件**
   - `README.md` 中的安装/使用说明
   - `docs/` 目录下的所有文档
   - CHANGELOG

3. **配置文件**
   - `.github/workflows/` CI 配置
   - Makefile 或 build scripts

4. **其他**
   - Git remote URL（如果同步改名）
   - Docker 相关配置

## 反模式

### 不要这样做：
- ❌ 只更新一个明显的文件就认为完成
- ❌ 假设 IDE 的"重命名重构"已经处理了所有引用
- ❌ 跳过 markdown 文档的检查

### 正确做法：
- ✅ 使用 grep 全局搜索旧名称
- ✅ 逐个确认并更新每个引用
- ✅ 验证新名称在关键位置都正确

## 本案例教训
项目 `ui-verify-ai` 重命名为 `ui-check` 时：
- 需要更新 `loader.go` 中的 `searchPaths`
- 需要更新 `README.md` 中的文档引用
- 使用 `grep -r "ui-verify-ai" .` 确认没有遗漏

## 工具选择理由
- **grep > IDE 重命名**：grep 可以搜索所有文件类型，包括 markdown、yaml
- **全局搜索 > 逐个检查**：效率更高，不易遗漏
