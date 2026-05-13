---
name: mcp-initialize-handshake
description: MCP 服务器需要先完成 initialize 握手才能处理 tools/list
metadata:
  category: case-study
  status: unverified
  created: "2026-04-19"
  verified_count: 0
---

# 案例：MCP Initialize 握手流程

## 症状

- MCP 服务器启动后立即发送 `tools/list` 请求无响应
- 连接似乎建立但没有任何返回
- 服务器日志显示请求已收到但未处理

## 根因

MCP 协议规定的握手流程：

1. **客户端** 发送 `initialize` 请求（包含 protocolVersion、capabilities、clientInfo）
2. **服务器** 返回 `serverInfo` 和 `capabilities`
3. **客户端** 发送 `initialized` 通知（无返回）
4. **之后** 服务器才能处理 `tools/list`、`resources/list` 等请求

跳过第 3 步直接发送 `tools/list` 会导致服务器忽略该请求。

## 正确流程

```
客户端 → 发送 initialize 请求
客户端 ← 接收 server info 响应
客户端 → 发送 initialized 通知（无返回）
客户端 → 发送 tools/list 请求 ← 此时服务器才会响应
```

## 代码示例

Go 中正确实现 MCP 握手：

```go
// 1. 等待 initialize 请求
var initReq mcp.InitializeRequest
decoder.Decode(&initReq)

// 2. 发送初始化响应
response := mcp.NewInitializeResult(
    initReq.Params.ProtocolVersion,
    mcp.ServerCapabilities{
        Tools: &mcp.ToolsCapability{},
    },
    mcp.ServerInfo{Name: "mcp-gateway", Version: "1.0.0"},
)
encoder.Encode(response)

// 3. 发送 initialized 通知（必须）
notification := mcp.NewInitializedNotification()
encoder.Encode(notification)

// 4. 现在可以处理 tools/list 等请求
```

## 教训

> MCP 协议要求严格的握手顺序，在完成 `initialize` + `initialized` 之前，服务器不会处理任何工具请求。