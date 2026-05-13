---
name: doc-implementation-mismatch
description: 架构文档与代码实现不一致导致的问题
metadata:
  category: anti-pattern
  status: unverified
  created: "2026-04-19"
  verified_count: 0
---

# 反模式：文档与实现不一致

## 识别信号

1. **语言/框架不匹配**
   - 文档描述 TypeScript/Node.js，实际是 Go 实现
   - 文档提到 `pool.ts` 但实际是 `pool.go`

2. **API 端点缺失或多余**
   - 文档定义 `POST /mcp`，实际是 `POST /messages`
   - 文档提到某端点但代码中搜索不到

3. **模块/组件缺失**
   - 文档描述的功能在代码中不存在
   - Stdio Bridge 文档有定义但代码中找不到

4. **路径结构差异**
   - 文档：`src/gateway/`
   - 实际：`src/pool/`

## 危害

1. **维护困惑** — 新成员按照文档开发，发现实现不一致
2. **错误估计** — 基于文档的工作量评估严重偏离实际
3. **集成失败** — 调用方按照文档对接，实际无法工作
4. **调试绕路** — 问题定位时文档指向错误位置

## 规避策略

1. **文档即代码**
   - 架构文档放在代码仓库中同步维护
   - 文档变更必须随代码变更一起提交

2. **代码优先文档**
   - 优先相信代码实现
   - 文档描述不清楚时，以代码为准

3. **定期审计**
   - 每次 release 前检查文档准确性
   - 使用工具检测文档中的路径/名称是否在代码中存在

## 正确做法

```
文档变更 → 必须同步代码
代码变更 → 必须更新文档
（任何单方面变更都是技术债）
```

## 检测命令

检查文档中定义的路径是否在代码中存在：

```bash
# 从文档提取路径，验证是否存在于代码库
grep -oE 'src/[a-zA-Z0-9/_.-]+\.(ts|go|py)' docs/*.md | while read path; do
  if [ ! -e "$path" ]; then
    echo "Missing: $path"
  fi
done
```