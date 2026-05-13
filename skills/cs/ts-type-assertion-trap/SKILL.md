---
name: ts-type-assertion-trap
description: TypeScript 类型断言掩盖运行时错误的案例
metadata:
  category: case-study
  status: unverified
  created: "2026-04-06"
  verified_count: 0
---

# 案例：TS 类型断言陷阱

## 症状
- TypeScript 编译通过（tsc 无报错）
- 运行时报错（TypeError、undefined 等）

## 定位过程

1. Grep 找到类型定义位置
2. Read 类型文件，发现使用了 `as any` 或 `as unknown as X` 强制断言
3. 追踪调用链，找到实际类型不匹配的位置
4. 确认运行时值与声明类型不一致

## 根因
使用 `as any` 等强制类型断言绕过了类型检查，掩盖了实际的类型不匹配。

## 解决方案

1. 移除 `as any`，使用 type guard（`if (typeof x === 'string')`）
2. 使用 Zod/io-ts 等运行时类型验证
3. 使用 `satisfies` 关键字替代断言

## 教训
`as any` 是代码异味。编译通过不代表运行时安全，优先使用 type guard。
