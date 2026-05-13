---
name: repeated-auth-retry
description: 认证失败时反复重试而不切换方案是低效的
metadata:
  category: anti-pattern
  status: draft
  created: "2026-04-07"
  verified_count: 0
---

# 反模式：反复重试已失败的认证方式

## 识别信号

- Token 认证失败但继续重试超过 2 次
- 错误信息一致（authentication failed）但仍尝试相同方式
- 检查了 token 格式、长度都正确，但仍不切换方案
- 没有尝试备选方案（如 SSH）就放弃推送

## 问题根因

当一种认证方式（TTPS + Token）持续失败时，存在两种低效行为：
1. **持续重试**：期望相同方式产生不同结果（精神熵）
2. **放弃操作**：直接告知用户无法推送而不提供替代方案

## 正确做法

### 立即切换备选方案

```bash
# 切换到 SSH 方式
git remote set-url origin git@github.com:<owner>/<repo>.git
git push
```

### 判断流程

```
认证失败 → 检查 token 格式是否正确
  ├── 格式错误 → 修复 token
  └── 格式正确 → 立即切换 SSH 方式
```

## 案例教训

- **问题**：Token 认证失败后反复检查 token 格式、重试推送，浪费 5 分钟
- **解决**：直接切换 SSH 方式，10 秒完成推送
- **结论**：有备选方案时，不要在死胡同里停留

## 正确心态

> 如果一条路走不通，就换一条路。不要在死胡同里反复尝试，期待它突然变成通途。
