---
name: search-first
description: 代码搜索类操作的优先级和最佳实践
metadata:
  category: methodology
  status: core
  verified_count: 3
---

# 搜索类工具优先级

## 优先级排序

1. **Grep** — 内容搜索首选
   - 支持正则表达式
   - 可按文件类型过滤（include 参数）
   - 返回文件路径和行号

2. **Glob** — 文件名模式匹配
   - 适用于"找到所有 XX 类型的文件"
   - 支持通配符（**/*.ts）
   - 按修改时间排序

3. **Read** — 读取特定已知文件
   - 用于你已经知道文件路径的情况
   - 支持 offset/limit 分段读取
   - 支持并行读取多个文件

4. **Task(explore)** — 复杂的开放式探索
   - 用于不确定要找什么的场景
   - 委托给 explore 子代理执行
   - 适合大面积代码库的初步了解

## 典型场景

### 场景 1：找到某个函数的定义
```
Grep("function myFunc|const myFunc|def myFunc")
→ 定位到文件和行号
→ Read 该文件对应行
```

### 场景 2：理解某个模块的架构
```
Glob("**/module-name/**/*.ts")
→ 了解文件结构
→ Read 入口文件（index.ts / main.ts）
→ Grep("export.*from", include="*.ts")  → 了解对外接口
```

### 场景 3：查找某个错误的来源
```
Grep("错误信息关键词")
→ 定位到出错文件
→ Read 出错上下文
→ 如需更深入，Grep 调用链
```

## 反模式
- 不要用 Read 逐个翻文件来"搜索"（效率极低）
- 不要在 Glob 能解决的问题上用 Task(explore)（杀鸡用牛刀）
- 不要忽略 Grep 的 include 参数（全文件搜索浪费时间）
