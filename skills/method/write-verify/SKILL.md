---
name: write-verify
description: write 工具写入文件后需要验证文件是否真正创建成功
metadata:
  category: methodology
  status: unverified
  created: "2026-04-19"
  verified_count: 0
---

# 流程：文件写入验证

## 背景

使用 `write` 工具创建新文件时，如果目标目录不存在或路径有误，写入操作可能静默失败。用户指出"文档还没建呢"说明文件并未真正创建。

## 标准流程

### 第一步：写入文件
使用 `write` 工具创建或覆盖文件。

### 第二步：验证文件存在
写入后立即使用 `bash ls` 或 `read` 工具验证文件是否真正创建：

```bash
# 验证文件是否存在
ls -la /path/to/file

# 或者读取文件内容确认
read /path/to/file
```

### 第三步：确认内容正确
读取文件内容，检查是否与预期一致。

## 识别信号

以下情况需要特别注意验证：
- 目标目录是新建的（需要先确认父目录存在）
- 写入新文件（非覆盖已有文件）
- 路径包含多层目录
- 用户明确表示"文档还没建呢"

## 正确做法示例

```
用户：创建 docs/ai-engineering/test.md

正确流程：
1. write 工具写入文件
2. bash ls 验证：ls docs/ai-engineering/test.md
3. 确认文件存在后再告知用户
```

## 反模式

- **不验证**：写入后直接告知用户完成，但文件实际不存在
- **假设成功**：认为 write 工具会自动创建目录
- **忽略用户反馈**：用户指出问题后仍不检查

## 相关经验
- method/bugfix-flow - Bug 修复后验证
- ap/skip-verify - 跳步验证反模式
