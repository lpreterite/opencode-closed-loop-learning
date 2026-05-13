---
name: https-to-ssh-fallback
description: GitHub HTTPS 认证失败时，切换到 SSH 方式是高效的备选路径
metadata:
  category: methodology
  status: draft
  created: "2026-04-07"
  verified_count: 0
---

# 工具优先级：GitHub 远端仓库协议切换

## 优先级排序

1. **HTTPS + Token** — 默认方式，配置简单
2. **SSH** — Token 认证失败时的备选方案

## 切换条件

当 `git push` 使用 HTTPS + Token 方式认证失败时，应立即切换到 SSH 方式，而非反复调试 token。

## 典型场景

### 场景：Token 认证持续失败

**信号：**
- `gh auth status` 显示正常但 `git push` 仍然失败
- Token 格式正确（ghp_ 开头），长度正常
- 反复重试仍无法认证

**操作步骤：**

```bash
# 1. 切换 remote URL 为 SSH 方式
git remote set-url origin git@github.com:<owner>/<repo>.git

# 2. 直接推送（使用 SSH key，无需 token）
git push
```

## 核心优势

- **无需重新认证**：SSH 使用 SSH key 而非 token
- **即时生效**：切换 URL 后即可推送
- **持久稳定**：SSH key 配置好后长期有效

## 反模式

不要在 token 认证持续失败时反复重试，应立即切换备选方案（SSH）。
