---
name: context-explosion
description: 上下文爆炸反模式 - 一次读取过多文件导致上下文浪费
metadata:
  category: anti-pattern
  status: core
  verified_count: 3
---

# 反模式：上下文爆炸

## 识别信号
- 一次读取超过 10 个文件
- 使用 Read 逐个"浏览"文件来找代码
- 加载了不相关的大文件（如 node_modules、dist）

## 规避策略

1. **先搜索再读取**
   - Grep 精确定位关键词 → 只读命中的文件
   - Glob 找到文件模式 → 只读关键文件

2. **分段读取**
   - 使用 Read 的 offset/limit 参数
   - 先读文件头（前 50 行）了解结构
   - 再定向读取关键段落

3. **委托子代理**
   - 用 Task(explore) 做大面积探索
   - 子代理有独立上下文，不影响主对话

4. **控制并行数量**
   - 同时读取不超过 5 个文件
   - 优先读取最可能相关的文件

## 正确做法
```
错误：Read 20个文件逐个浏览
正确：Grep("关键词") → Read 3个命中的文件
```
