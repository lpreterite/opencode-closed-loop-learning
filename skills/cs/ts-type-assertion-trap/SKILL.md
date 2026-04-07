---
name: ts-type-assertion-trap
description: TypeScript 类型断言掩盖运行时错误
metadata:
  category: case-study
  status: verified
  created: "2026-04-07"
  apply_count: 0
  fail_count: 0
  last_fail: null
  corrections: []
---

# 案例：TS 类型断言掩盖运行时错误

## 背景

TypeScript 编译通过，但运行时出错。问题根源是类型断言（`as`）掩盖了实际的类型不匹配。

## 症状

```typescript
// 编译通过，运行时崩溃
const data = response.data as SomeType
data.items.forEach(...) // Cannot read property 'forEach' of undefined
```

## 定位过程

```
1. 搜索 "as" 查看所有类型断言
2. 检查断言前后的类型是否真的匹配
3. 用 console.log 或调试确认实际数据结构
```

## 根因

`response.data` 的类型定义是 `SomeType`，但实际返回可能是：
- `null`
- `{ items: undefined }`
- 类型完全不匹配

**类型断言跳过编译期检查，欺骗了类型系统。**

## 解决方案

### 方案一：不用断言，用类型守卫

```typescript
if (response.data && Array.isArray(response.data.items)) {
  response.data.items.forEach(...)
}
```

### 方案二：声明更精确的类型

```typescript
type DataType = {
  items: Item[] | undefined
}
```

### 方案三：运行时验证

```typescript
function isSomeType(obj: unknown): obj is SomeType {
  return obj !== null && typeof obj === 'object' && 'items' in obj
}
```

## 适用边界

**这类问题在什么情况下适用？**
- TypeScript 编译通过但运行时报错
- 使用了大量 `as` 类型断言
- API 返回数据结构不确定

**什么情况下不适用？**
- 纯 JavaScript 项目
- 确认类型一定匹配的简单场景

## 教训

**类型断言是短路开关，使用时必须确认类型一定匹配。**
