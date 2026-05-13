---
name: ci-test-timeout
description: CI 环境中测试超时但本地正常的诊断与定位
metadata:
  category: case-study
  status: unverified
  created: "2026-04-18"
  verified_count: 0
---

# 案例：CI 测试超时问题

## 症状

- CI 环境中测试失败，错误信息显示 **超时（600 秒）**
- 失败测试：`TestGracefulShutdownChannel`
- 本地运行相同测试可能正常通过
- 重试 CI 可能仍然超时

## 定位过程

### 第一步：通过 gh 命令获取 CI 日志

```bash
gh run list --limit 5
gh run view <run-id> --log
```

从日志中筛选关键错误信息：
```bash
gh run view <run-id> --log | grep -A 5 "FAIL\|TIMEOUT\|Error"
```

### 第二步：分析超时原因

常见超时场景：
1. **资源限制** — CI 环境 CPU/内存受限，并发测试争抢资源
2. **通道阻塞** — 测试中的 channel 操作在 CI 环境下行为不同
3. **时序敏感** — 依赖 sleep/wait 的测试在 CI 中执行速度不稳定
4. **文件描述符限制** — CI 环境默认限制可能影响网络/文件操作

### 第三步：验证假设

- 在本地运行相同测试多次，确认是否稳定通过
- 检查测试是否有时序依赖（如 sleep、time.Sleep）
- 查看是否有资源争用（并发测试、共享状态）

## 根因

`TestGracefulShutdownChannel` 是一个时序敏感的测试，依赖通道操作和 goroutine 调度。在 CI 环境的负载下，600 秒的超时限制不足以完成测试。

## 解决方案

### 方案 1：增加超时容差

如果测试框架支持，可增加超时配置：

```go
// 在测试中调整上下文超时
ctx, cancel := context.WithTimeout(ctx, 30*time.Second) // 而不是更短
```

### 方案 2：优化测试性能

- 减少不必要的 sleep
- 使用 channel 同步替代 time.Sleep 等待
- 降低测试并发度

### 方案 3：CI 配置调优

```yaml
# .github/workflows/ci.yml
jobs:
  test:
    runs-on: ubuntu-latest
    timeout-minutes: 15  # 确保超时设置足够长
```

## 教训

> CI 环境与本地环境存在性能差异，时序敏感的测试需要设计得更健壮，或在 CI 中分配更多资源。

## 预防措施

1. 测试应该有明确的完成信号，而不是依赖固定等待时间
2. 使用 `context.WithTimeout` 替代 `time.Sleep` 进行等待
3. 在 CI 环境中运行压力测试，尽早发现资源争用问题
