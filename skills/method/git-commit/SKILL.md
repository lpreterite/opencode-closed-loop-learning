---
name: git-commit
description: Git 提交和 PR 操作的最佳实践
metadata:
  category: methodology
  status: core
  verified_count: 3
---

# Git 操作流程

## 提交前检查

1. git status — 查看所有变更
2. git diff — 审查具体改动
3. 确认没有遗漏敏感信息（.env、密钥等）

## Commit 规范

- 只在用户明确要求时才 commit
- 提交信息简洁，聚焦"为什么"而非"做了什么"
- 格式：`<类型>: <简要描述>`
  - feat: 新功能
  - fix: 修复
  - refactor: 重构
  - docs: 文档
  - test: 测试

## 禁止操作

- 不要 force push（除非用户明确要求）
- 不要修改 git config
- 不要跳过 hooks（--no-verify）
- 不要 amend 已推送的 commit

## PR 创建

- 使用 gh CLI 工具
- 标题和正文清晰描述变更
- 引用相关 issue
