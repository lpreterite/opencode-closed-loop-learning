---
name: service-start-short-circuit
description: macOS service start 命令无输出且 launchd 注册失败的根本原因分析
metadata:
  category: case-study
  status: unverified
  created: "2026-04-19"
  verified_count: 0
---

# 案例：service start 短路逻辑问题

## 症状

```bash
$ mcp-gateway service start
# 命令执行后没有任何输出
# launchd 注册失败，服务无法启动
```

## 定位过程

### 1. 问题复现
- 执行 `brew install` 后运行 `mcp-gateway service start`
- 命令挂在后台但无任何日志输出

### 2. 启动模式分析
- 手动调用 `mcp-gateway --mode service start`
- 仍然无输出

### 3. 代码审查（通过 Tester subagent）
- 检查 `platform_darwin.go` 中的 `Start()` 函数
- 发现短路逻辑问题

## 根因

在 `platform_darwin.go` 的 `Start()` 函数中存在错误的条件判断：

```go
// 错误示例：serviceName 为空时提前返回
func (s *Service) Start() error {
    if s.Config.serviceName == "" {  // 这里短路了实际启动逻辑
        return nil  // 看似成功，实际什么都没做
    }
    // ... 实际启动逻辑永远执行不到
}
```

## 解决方案

移除不必要的短路逻辑，确保启动流程正常执行：

```go
// 正确示例：直接执行启动逻辑
func (s *Service) Start() error {
    // 直接执行 launchd 注册和启动
    return s.launchctlStart()
}
```

## 教训

- **短路逻辑需谨慎**：提前返回的判断条件必须确保是正确的业务逻辑
- **无输出 ≠ 成功**：command 没有输出不代表执行成功，可能是在某个判断点被短路了
- **启动类命令需要日志**：service start 这类关键操作应该有明确的成功/失败日志
