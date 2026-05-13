---
name: multi-subagent-collaboration
description: Tester 发现问题后转交 Developer 修复的标准协作流程
metadata:
  category: methodology
  status: unverified
  created: "2026-04-19"
  verified_count: 0
---

# 多 Subagent 协作流程

## 协作架构

```
┌─────────────┐     发现问题      ┌─────────────┐
│   Tester    │ ───────────────→ │  Developer  │
│  (测试工程师) │   提供完整诊断    │  (开发工程师) │
└─────────────┘                   └─────────────┘
```

## 标准流程

### 第一步：Tester 问题发现与诊断
1. 复现问题场景
2. 定位问题根因
3. **输出结构化的诊断报告**，包括：
   - 问题现象
   - 错误位置（文件 + 函数）
   - 根因分析
   - 建议修复方案

### 第二步：转交 Developer
1. Tester 完成任务分析但不自行修复
2. 将诊断报告完整传递给 Developer
3. Developer 根据诊断报告实施修复

### 第三步：Developer 修复与验证
1. 根据诊断报告修复代码
2. 运行 lint 检查代码质量
3. 运行 test 验证功能正确性
4. 运行 build 确保编译通过

### 第四步：验证闭环
1. 确认问题已解决
2. 确认无引入新问题

## 关键原则

- **诊断与执行分离**：Tester 负责找问题，Developer 负责解决问题
- **信息传递完整性**：诊断报告必须包含足够的上下文，避免 Developer 重复诊断
- **验证必要性**：修复后必须运行完整的验证流程（lint + test + build）

## 适用场景

- 复杂 bug 需要专业测试能力定位
- 问题根因不明确需要多轮探索
- 需要明确分工的专业化协作
