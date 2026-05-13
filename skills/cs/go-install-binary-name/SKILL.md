---
name: go-install-binary-name
description: Go install 安装的二进制名由模块名决定，非 cmd 目录名
metadata:
  category: case-study
  status: unverified
  created: "2026-04-11"
  verified_count: 0
---

# 案例：Go install 二进制名与 cmd 目录名不一致

## 症状
执行 `go install github.com/lpreterite/ui-check@latest` 后：
- 期望的二进制名：`ui-check`
- 实际生成的二进制名：`cli`
- 运行 `ui-check` 命令报错：command not found
- 运行 `cli` 命令正常

## 定位过程
1. 检查 `go env GOBIN` 确认安装目录
2. `ls ~/go/bin/` 发现实际生成的是 `cli` 而非 `ui-check`
3. 检查模块名：模块声明为 `module github.com/lpreterite/ui-check`
4. 检查 cmd 目录：cmd 目录下是 `cli` 目录

## 根因
`go install` 生成二进制名的规则：
- **二进制名 = 模块名最后一个路径组件**，而非 cmd 下的目录名
- 本例：模块名 `github.com/lpreterite/ui-check` → 二进制名取 `ui-check`
- 但实际行为显示：go install 取的是 **main package 名**（cmd/cli/main.go 中 package main 所属的目录名）

实际上更准确的说法是：`go install` 会使用你指定的导入路径的最后一个组件。

## 解决方案

### 方案一：创建符号链接（快速解决）
```bash
ln -sf ~/go/bin/cli ~/go/bin/ui-check
```

### 方案二：使用 -o 指定输出名（推荐）
```bash
go install github.com/lpreterite/ui-check@latest -o ui-check
```

### 方案三：修改模块路径
确保模块名与期望的二进制名一致。

## 教训
1. **安装后立即检查实际生成的二进制名称**，不要假设
2. 使用 `go install -o` 可以显式指定输出文件名
3. 查看 `go env GOBIN` 和 `ls $(go env GOBIN)` 验证安装结果

## 预防
在执行 `go install` 前，查阅 `go help install` 了解确切行为：
- 二进制名由导入路径的最后一个组件决定
- 可以用 `-o` 标志覆盖默认行为
