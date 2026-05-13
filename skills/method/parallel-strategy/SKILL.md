---
name: parallel-strategy
description: 多工具并行执行策略，优化上下文效率
metadata:
  category: methodology
  status: core
  verified_count: 3
---

# 并行执行策略

## 核心原则

在同一消息中发起多个独立的工具调用，减少往返次数。

## 可以并行的操作

| 操作组合 | 示例 |
|---------|------|
| 多个 Grep 搜索 | 同时搜索多个关键词 |
| 多个 Glob 匹配 | 同时匹配多种文件模式 |
| 多个 Read 读取 | 同时读取多个不相关文件 |
| Grep + Glob | 同时搜索内容和匹配文件名 |

## 不能并行的操作

| 操作组合 | 原因 |
|---------|------|
| Read + Edit 同一文件 | Edit 依赖 Read 的结果 |
| Edit + Edit 同一文件 | 行号可能偏移 |
| 依赖前一步结果的任何操作 | 需要等待结果 |

## 典型模式

### 模式 1：快速代码定位
```
单条消息中并行：
  Grep("TODO", include="*.ts")
  Grep("FIXME", include="*.ts")
  Glob("**/*.test.ts")
→ 一次获得所有结果
```

### 模式 2：理解模块
```
单条消息中并行：
  Glob("**/auth/**/*.ts")
  Grep("export.*auth", include="*.ts")
  Read("src/auth/index.ts")
→ 一次获得文件列表、接口和入口
```

## 反模式
- 不要逐个文件 Read（应批量并行）
- 不要先 Glob 再逐个 Grep（应并行发起）
- 不要在可以并行时串行执行独立搜索
