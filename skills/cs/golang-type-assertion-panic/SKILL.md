---
name: golang-type-assertion-panic
description: Golang 类型断言 Panic 导致 HTTP 连接崩溃的案例
metadata:
  category: case-study
  status: unverified
  created: "2026-04-19"
  verified_count: 0
---

# 案例：Golang 类型断言 Panic

## 症状
- HTTP 连接突然崩溃
- 服务出现 "connection reset by peer" 或类似错误
- 没有任何业务逻辑错误，但请求直接失败

## 定位过程

1. **查看错误日志** - 找到 panic 相关的 stack trace
2. **定位出错位置** - 搜索 panic 发生点，通常在调用 `.(Type)` 类型断言的地方
3. **追踪数据类型** - 检查函数返回值的实际类型 vs 期望类型
4. **检查接口定义** - 确认实现类是否正确实现了接口

## 根因

`MCPClientConnection.CallTool` 返回类型为 `map[string]interface{}`，但调用方错误地将其类型断言为 `*ToolCallResult`。

```go
// 错误示例
result := conn.CallTool(name, args).(*ToolCallResult)  // Panic!

// 正确做法
result := conn.CallTool(name, args)
if tr, ok := result.(*ToolCallResult); ok {
    // 安全使用
}
```

## 解决方案

1. **修改函数返回类型** - 让 `CallTool` 直接返回正确的结构体类型 `*ToolCallResult`
2. **内部解析** - 在函数内部将 `map[string]interface{}` 解析为正确的结构体
3. **使用 type switch** - 如果确实需要返回 interface{}，调用方应使用 type switch 安全处理

## 教训

Golang 的类型断言（`x.(T)`）如果失败会直接 panic，而非返回错误。对于从外部输入（HTTP 响应、文件读取等）获取的数据，**永远不要直接使用类型断言**，而应该：
- 使用 `ok` 模式：`if v, ok := x.(T); ok { ... }`
- 使用 type switch：`switch v := x.(type) { case T1: ... case T2: ... }`
- 使用反射（`reflect` 包）进行安全类型转换

## 相关反模式
- ap/skip-verify - 修改后未验证
- ap/overedit - 一次修改过多文件
