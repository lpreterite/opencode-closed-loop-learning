---
name: edit-over-write
description: 文件编辑类操作的优先级和最佳实践
metadata:
  category: tool-priority
  status: unverified
  verified_count: 0
---

# 编辑类工具优先级

## 优先级排序

1. **Edit** — 修改已有文件的首选
   - 精确字符串替换
   - 支持replaceAll批量替换
   - 保留文件其他部分不变

2. **Write** — 仅用于以下场景
   - 创建全新文件
   - 文件需要全量重写（超过50%内容变更）
   - 格式化/美化整个文件

3. **Bash(sed/awk)** — 最后手段
   - 仅当 Edit 无法满足时
   - 批量文件处理（如重命名）
   - 流式文本处理

## 使用 Edit 的关键规则

1. 先 Read 文件，再 Edit（确保 oldString 精确匹配）
2. oldString 必须是原文的精确复制（包括缩进和空行）
3. 提供足够上下文使 oldString 唯一（避免多处匹配）
4. 如果需要替换多处相同内容，使用 replaceAll: true

## 典型场景

### 场景 1：修改函数实现
```
Read(filePath) → 看到函数完整代码
Edit(filePath, oldString="旧函数体", newString="新函数体")
```

### 场景 2：重命名变量
```
Read(filePath) → 确认变量名
Edit(filePath, oldString="oldName", newString="newName", replaceAll=true)
```

### 场景 3：创建新组件
```
Write(filePath, content="完整文件内容")
```
