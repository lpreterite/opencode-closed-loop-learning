---
name: experience-index
description: 经验路由索引 - 根据当前任务类型匹配并推荐应加载的领域经验
metadata:
  category: index
  status: core
---

# 经验路由索引

## 使用方式
这是经验系统的入口。加载后，根据当前任务类型，推荐下一步加载的具体经验。

## 工具优先级经验 (tp/)

| 场景 | 推荐加载 | 状态 |
|------|---------|------|
| 代码搜索/定位 | tp/search-first | ✅核心 |
| 文件编辑/修改 | tp/edit-over-write | ⚡待验证 |
| 多文件并行操作 | tp/parallel-strategy | ✅核心 |
| Pencil 设计修复 | tp/pencil-mcp-workflow | 使用 Pencil VS Code 扩展时 | ⚡待验证 |
| 设计验证降级策略 | tp/design-verification-fallback | 图像识别工具不可用 / "Not connected" / 模型不支持图像 | ✅已验证 |
| GitHub SSH 切换 | tp/https-to-ssh-fallback | git push 认证失败 / token 格式正确但仍无法推送 | ⚡待验证 |

## 流程经验 (wf/)

| 流程 | 推荐加载 | 触发条件 | 状态 |
|------|---------|---------|------|
| 新功能开发 | wf/feature-dev | 用户要求添加新功能 | ✅核心 |
| Bug 修复 | wf/bugfix-flow | 用户报告错误/异常 | ✅核心 |
| 代码重构 | wf/refactor-safe | 用户要求改善代码结构 | ⚡待验证 |
| Git 操作 | wf/git-commit | 涉及 commit/PR 操作 | ✅核心 |

## 案例经验 (cs/)

| 案例 | 推荐加载 | 信号 | 状态 |
|------|---------|------|------|
| 依赖版本冲突 | cs/dep-version-conflict | module not found / 版本报错 | ⚡待验证 |
| TS 类型断言陷阱 | cs/ts-type-assertion-trap | 编译通过但运行报错 | ⚡待验证 |
| Pencil flexbox 修复 | cs/pencil-flexbox-fix | flexbox 布局中子节点位置偏离/裁剪 | ⚡待验证 |
| Pencil 图标替换 | cs/pencil-icon-repair | 空图标需替换为 Lucide icon_font / 设计组件图标缺失 | ✅已验证 |

## 反模式警告 (ap/)

| 反模式 | 推荐加载 | 信号 | 状态 |
|--------|---------|------|------|
| 上下文爆炸 | ap/context-explosion | 一次读取超过 10 个文件 | ✅核心 |
| 过度编辑 | ap/overedit | 单次修改超过 5 个文件 | ✅核心 |
| 跳步验证 | ap/skip-verify | 修改完未运行 lint/test | ✅核心 |
| 反复认证重试 | ap/repeated-auth-retry | 认证失败后持续重试而不切换备选方案 | ⚡待验证 |

## 加载策略
1. 只加载与当前任务直接相关的 1-2 条经验
2. 经验间有交叉时，优先加载 workflow 类型
3. 完成后不需要主动卸载（compaction 会自动清理）
