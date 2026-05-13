---
name: go-subprocess-stdin-delay
description: Go subprocess 启动后需等待约 100ms 才能接收输入
metadata:
  category: case-study
  status: unverified
  created: "2026-04-19"
  verified_count: 0
---

# 案例：Go subprocess 启动延迟问题

## 症状

- Go 程序启动 MCP 服务器子进程后，立即发送输入但服务器无响应
- 增加延迟（如 sleep 1秒）后正常工作
- 服务器日志显示"请求未收到"

## 根因

进程启动和 IO 初始化需要时间：
1. fork/exec 系统调用开销
2. 标准库初始化
3. 子进程 stdio buffer 准备好
4. 操作系统进程调度

通常需要 **50-150ms** 才能稳定接收输入。

## 解决方案

### 方案 1：固定延迟等待

```go
cmd := exec.Command("npx", "-y", "@modelcontextprotocol/server-filesystem", "/tmp")
cmd.Stdin = os.Stdin
cmd.Stdout = os.Stdout
cmd.Stderr = os.Stderr
cmd.Start()

// 等待进程启动完成
time.Sleep(100 * time.Millisecond)

// 现在可以发送数据
```

### 方案 2：等待进程就绪信号

更可靠的方式是等待子进程自己准备好：

```go
cmd := exec.Command("npx", "-y", "@modelcontextprotocol/server-filesystem", "/tmp")
stdout, _ := cmd.StdoutPipe()
cmd.Start()

// 读取一行输出作为就绪信号
buf := make([]byte, 1)
stdout.Read(buf)

// 现在可以发送数据
```

### 方案 3：检查进程是否可写

使用轮询检测：

```go
cmd := exec.Command("npx", "-y", "@modelcontextprotocol/server-filesystem", "/tmp")
cmd.Stdin = os.Stdin
cmd.Stdout = os.Stdout
cmd.Start()

// 等待进程可写
for i := 0; i < 100; i++ {
    f, err := os.Create("/dev/null")
    if err == nil {
        f.Close()
        break
    }
    time.Sleep(10 * time.Millisecond)
}
```

## 教训

> Go subprocess 启动后不应立即使用，需要等待进程初始化完成。100ms 是安全的延迟值。