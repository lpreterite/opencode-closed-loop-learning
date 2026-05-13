---
name: opencode-annotations-null
description: opencode MCP 工具列表返回 annotations: null 导致校验失败的案例
metadata:
  category: case-study
  status: unverified
  created: "2026-04-19"
  verified_count: 0
---

# 案例：opencode MCP annotations null 校验失败

## 症状
- `opencode mcp list` 显示 `Failed to get tools`
- opencode 连接状态显示 `connected` 但实际无法获取工具列表
- MCP 服务端日志无明显错误

## 定位过程

1. **复现命令验证**
   ```bash
   opencode mcp list
   # 输出显示 connected 但获取工具失败
   ```

2. **检查 MCP 服务响应**
   - 手动构造请求调用 `/tools/list` 端点
   - 发现响应中包含 `"annotations": null`

3. **深入分析**
   - opencode 对 MCP 协议字段有严格校验
   - `annotations` 字段必须是 `object` 类型或省略
   - 当 `annotations` 为 `null` 时，opencode 校验失败
   - 错误信息：`Invalid input: expected object, received null`

## 根因
MCP 服务端在 `tools/list` 响应中返回了 `"annotations": null`，而 opencode 客户端要求该字段必须是对象或不存在，不能是 null 值。

## 解决方案

在 server.go 中，仅在 annotations 非 nil 时才返回该字段：

```go
// 修复前
"annotations": annotation,

// 修复后
if annotation != nil {
    tool["annotations"] = annotation
}
```

## 验证

修复后执行：
```bash
opencode mcp list
# 应显示 connected，获取工具列表正常
```

## 教训

1. **不能靠猜测定位根因**：第一次修复失败是因为未聚焦核心问题，未验证日志
2. **聚焦 + 日志验证是关键**：第二次修复成功是因为直接检查了 MCP 响应内容
3. **严格协议字段校验**：MCP 客户端对字段类型有严格要求，null vs 省略有区别

## 相关错误信号
- "Failed to get tools"
- "Invalid input: expected object, received null"
- MCP 服务运行正常但客户端无法获取工具列表
