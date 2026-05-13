---
name: opencode-mcp-debug
description: 使用 opencode mcp 命令调试 MCP 服务连接问题
metadata:
  category: tool-priority
  status: unverified
  created: "2026-04-19"
  verified_count: 0
---

# opencode mcp 命令调试指南

## 常用命令

### 查看 MCP 连接状态
```bash
opencode mcp list
```
- 显示所有已配置的 MCP 服务及其连接状态
- `connected` = 正常
- `failed` = 连接失败

### 测试 MCP 服务端点
```bash
# 手动调用 tools/list 端点检查响应
curl -X POST http://localhost:<port>/tools/list \
  -H "Content-Type: application/json" \
  -d '{}'
```

## 调试步骤

### 第一步：确认连接状态
```bash
opencode mcp list
```
- 如果显示 `connected` 但获取工具失败 → 检查服务端响应格式
- 如果显示 `failed` → 检查 MCP 服务是否启动、网络是否通

### 第二步：检查服务端日志
- 查看 MCP 服务端是否有错误日志
- 注意 `annotations` 字段是否为 `null`

### 第三步：验证响应格式
MCP 服务 `/tools/list` 响应必须符合规范：
- `tools` 数组
- 每个 tool 的 `annotations` 必须是 `object` 或省略，**不能是 `null`**

### 第四步：测试工具调用
```bash
opencode mcp call <tool-name> <args>
```
- 验证单个工具是否能正常调用

## 常见问题

| 问题 | 原因 | 解决方案 |
|------|------|---------|
| `Failed to get tools` | annotations 为 null | 服务端仅在非 nil 时返回该字段 |
| `connected` 但无工具 | 服务端响应格式错误 | 检查 JSON 结构 |
| 连接超时 | 服务未启动或端口不通 | 确认服务启动 `ps aux \| grep <服务名>` |

## 关键检查点

1. MCP 服务是否运行：`ps aux | grep mcp`
2. 服务端口是否可达：`curl http://localhost:<port>/health`
3. 响应格式是否正确：特别是 `annotations` 字段类型
