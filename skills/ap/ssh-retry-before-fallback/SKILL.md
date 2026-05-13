---
name: ssh-retry-before-fallback
description: SSH 连接失败时先重试再考虑切换方案，不要立即切换到 HTTPS
metadata:
  category: anti-pattern
  status: unverified
  created: "2026-04-11"
  verified_count: 0
---

# 反模式：SSH 连接失败后立即切换 HTTPS 而非重试

## 识别信号
- SSH 推送时报错：`Connection closed by 20.205.243.166 port 22`
- 首次推送失败后立即尝试切换到 HTTPS 方案
- 没有等待一段时间就直接放弃 SSH

## 规避策略

### 1. 区分认证失败 vs 网络问题
| 错误类型 | 特征 | 正确做法 |
|---------|------|---------|
| 认证失败（公钥/密钥问题） | "Permission denied" / "Authentication refused" | 检查 SSH key 配置，考虑切换 |
| **网络问题** | "Connection closed by remote" / 超时 | **等待后重试** |
| 防火墙/端口阻塞 | 连接被拒绝 | 检查网络配置 |

### 2. 重试策略
```
第一次失败 → 等待 5-10 秒 → 重试
第二次失败 → 等待 30 秒 → 重试  
第三次失败 → 等待 1 分钟 → 重试
如果持续失败 → 再考虑切换方案
```

### 3. SSH 连接测试命令
在执行 git push 前，先测试 SSH 连接：
```bash
ssh -T git@github.com
ssh -vvv git@github.com  # 详细调试
```

## 正确做法

### 网络问题导致 SSH 失败时
1. **先等待再重试**（网络波动可能自行恢复）
2. 检查本地网络状态
3. 确认 GitHub 状态页面（downdetector）
4. 只有在多次重试仍失败后才考虑切换到 HTTPS

### 本案例教训
本次对话中 SSH `Connection closed by 20.205.243.166 port 22`：
- 首次失败后立即考虑切换 HTTPS
- 实际上等待约 1 分钟后重试成功
- **教训**：网络瞬断不要立即切换，先重试

## 相关经验
- 备选方案切换：tp/https-to-ssh-fallback（SSH 真正失败时的备选）
