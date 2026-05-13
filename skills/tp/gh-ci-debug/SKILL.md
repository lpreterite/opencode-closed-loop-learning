---
name: gh-ci-debug
description: 使用 gh 命令查看 GitHub Actions CI 运行状态和日志
metadata:
  category: tool-priority
  status: unverified
  created: "2026-04-18"
  verified_count: 0
---

# GitHub CI 调试工具优先级

## 优先级排序
1. **gh run list** — 查看最近 CI 运行列表
2. **gh run view \<run-id\> --log** — 查看指定运行的完整日志
3. **gh run watch** — 实时监控运行状态（可选）

## 典型场景

### 场景：调试 CI 失败原因

当 CI 失败需要定位问题时，使用 gh 命令快速获取信息：

```bash
# 1. 查看最近 5 次运行
gh run list --limit 5

# 2. 查看指定运行的完整日志（包含每一步输出）
gh run view <run-id> --log

# 3. 查看运行详细信息（包括时长、触发者）
gh run view <run-id>
```

### 场景：筛选特定状态的运行

```bash
# 只看失败的运行
gh run list --status failure --limit 10

# 只看特定 workflow
gh run list --workflow "CI"
```

## 关键命令示例

| 操作 | 命令 |
|------|------|
| 列出最近运行 | `gh run list --limit 5` |
| 查看运行详情 | `gh run view <run-id>` |
| 获取完整日志 | `gh run view <run-id> --log` |
| 实时监控 | `gh run watch <run-id>` |

## 注意事项

- `gh run view <run-id> --log` 会返回完整的日志输出，适合分析失败原因
- 运行 ID 可以从 `gh run list` 的输出中获取
- 日志会显示每个 step 的独立输出，方便定位具体哪一步失败

## 优势对比

相比登录 GitHub 网页查看 CI：
- 无需打开浏览器
- 可直接在终端中grep/cat分析日志
- 适合集成到脚本中自动化
