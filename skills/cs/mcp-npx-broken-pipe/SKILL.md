---
name: mcp-npx-broken-pipe
description: npx -y 启动的 MCP 服务器当 stdin 被 pipe 时会立即断开连接
metadata:
  category: case-study
  status: unverified
  created: "2026-04-19"
  verified_count: 0
---

# 案例：npx broken pipe 问题

## 症状

- 使用 `npx -y @xxx` 启动 MCP 服务器时，服务器立即断开连接
- 错误信息可能包含 `broken pipe` 或连接被关闭
- 手动在终端运行 npx 命令正常，但通过 subprocess 调用时失效

## 根因

`npx` 在执行时默认会检查 stdin 是否为 TTY（终端）。当通过 subprocess 的 pipe 方式启动时：
- stdin 被重定向为 pipe 而非终端
- npx 误判为非交互式环境，直接退出
- MCP 服务器进程也随之终止

## 解决方案

### 方案 1：直接调用二进制

不通过 npx，直接安装并调用 MCP 服务器的二进制：

```bash
# 安装到本地
npm install -g @modelcontextprotocol/server-filesystem

# 直接调用
/node/path/to/mcp-server-filesystem /path/to/directory
```

### 方案 2：使用 pnpm exec

```bash
pnpm exec --yes @modelcontextprotocol/server-filesystem /path/to/directory
```

### 方案 3：配置 subprocess 伪终端

在 Go 中启动 subprocess 时，配置 `SysProcAttr` 启用伪终端：

```go
cmd := exec.Command("npx", "-y", "...")
cmd.SysProcAttr = &syscall.SysProcAttr{
    Setctty: true,
    Setsid:  true,
}
```

### 方案 4：检查 MCP 配置

对于 OpenCode MCP 配置，优先使用直接二进制路径而非 npx 调用：

```json
{
  "mcp": {
    "servers": {
      "filesystem": "/usr/local/bin/mcp-server-filesystem"
    }
  }
}
```

## 教训

> 使用 `npx -y` 启动 MCP 服务器在 subprocess 中不可靠，应直接使用二进制或配置伪终端。