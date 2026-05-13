---
name: skip-verify
description: 跳步验证反模式 - 修改后未运行 lint/test 验证
metadata:
  category: anti-pattern
  status: core
  verified_count: 4
---

# 反模式：跳步验证

## 识别信号
- 修改完代码直接报告"完成"
- 未运行项目配置的 lint/typecheck
- 未运行测试套件
- 自认为"应该没问题"

## 规避策略

1. **修改后必验证**
   - 代码修改后运行 lint
   - 代码修改后运行 typecheck（如适用）
   - 如有测试，运行相关测试

2. **验证失败必须处理**
   - lint 错误：修复
   - typecheck 错误：修复
   - 测试失败：分析原因并修复

3. **向用户报告验证结果**
   - 明确说明验证了什么
   - 报告验证结果（通过/失败）

## 验证命令优先级
1. 查看 package.json 的 scripts 字段
2. 查找 Makefile
3. 查找 README 中的说明
4. 直接询问用户
