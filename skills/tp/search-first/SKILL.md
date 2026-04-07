---
name: search-first
description: 搜索优先原则 - 动手前先搜索了解现状
metadata:
  category: tool-priority
  status: verified
  created: "2026-04-07"
  apply_count: 0
  fail_count: 0
  last_fail: null
  corrections: []
---

# 搜索优先原则

## 优先级排序
1. **grep/ripgrep** — 代码内容搜索，最快定位关键词
2. **glob** — 按文件名模式搜索，适合找特定类型文件
3. **read** — 确认文件内容，验证搜索结果
4. **edit/write** — 修改文件，最后一步

## 适用边界

**适用场景：**
- 需要找文件位置或代码位置时
- 不确定某个功能在哪个文件时
- 需要了解现有代码结构时

**不适用场景：**
- 已知具体文件路径，直接 read/edit
- 明确知道要创建新文件

## 典型场景

### 场景：不确定某个函数在哪里

```
1. grep 搜索函数名
2. 找到后 read 确认上下文
3. 如需修改，再用 edit
```

### 场景：需要了解项目结构

```
1. glob "**/*.ts" 列出所有 TypeScript 文件
2. 根据文件名作判断
3. 针对性地 read 需要了解的
```

## 反模式

- **上来就 read 整个目录** — 浪费上下文，应该用 glob 筛选
- **用 Bash find 命令** — 应该用内置的 glob 工具
- **不搜索直接问用户** — 用户不一定记得，应该先自己搜索

## 失败案例

AI 上来就读了 20 个文件来"了解项目"，导致上下文爆炸。正确做法是先 grep/glob 定位关键文件。
