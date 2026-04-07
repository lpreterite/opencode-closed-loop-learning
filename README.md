# OpenCode 闭环学习系统

> Closed-Loop Learning System for OpenCode

**所属目录**：`opencode-closed-loop-learning/`
**文档状态**：设计中
**当前版本**：v0.2
**最后更新**：2026-04-07

---

## 概述

本目录收录 **OpenCode 闭环学习系统**的完整设计与实现文档，实现 AI 编程助手的**按需加载经验**和**半自动沉淀**能力：

- 按需加载：根据任务类型自动匹配并加载相关经验
- 经验分类：工具优先级、流程、案例、反模式四类
- 闭环沉淀：从对话中提取有价值经验，持续积累
- 质量验证：新经验需经过验证才标记为可靠

---

## 目录结构

```
opencode-closed-loop-learning/
├── README.md
├── STATUS.md
│
├── guide/                          # 系统设计文档
│   ├── 01-architecture.md          # 系统架构、四层模型、闭环流程
│   ├── 02-usage.md                # 日常使用指南、命令参考
│   ├── 03-experience-management.md # 经验状态流转、质量验证机制
│   └── 04-deployment.md           # 部署指南、配置说明
│
├── skills/                         # 经验库（实时同步到 ~/.config/opencode/skills/）
│   └── experience-index/           # 经验路由索引（入口）
│
├── agents/                         # Agent 角色定义
│   └── experience-miner.md        # 经验矿工 Agent
│
└── setup/                         # 部署指南
    └── opencode.md                # OpenCode 安装配置
```

---

## 文档索引

### 设计文档（guide/）

| 文档 | 说明 | 状态 |
|------|------|------|
| [01-architecture.md](./guide/01-architecture.md) | 系统架构：四层模型、闭环流程、OpenCode 特性映射 | ✅ 完成 |
| [02-usage.md](./guide/02-usage.md) | 日常使用：命令参考、加载策略、经验积累节奏 | ✅ 完成 |
| [03-experience-management.md](./guide/03-experience-management.md) | 经验管理：状态流转、质量验证、清理机制 | ✅ 完成 |
| [04-deployment.md](./guide/04-deployment.md) | 部署指南：配置文件说明、目录结构、验证步骤 | ✅ 完成 |

### 经验库（skills/）

> 经验库文件实时同步至 `~/.config/opencode/skills/`。用户通过 `/mine` 命令沉淀经验，详见 [03-experience-management.md](./guide/03-experience-management.md)。

| 类别 | 目录 | 说明 | 状态 |
|------|------|------|------|
| 经验索引 | `experience-index/` | 经验路由入口，按任务类型推荐 | ✅ 核心 |
| 工具优先级 | `tp/` | 搜索、编辑、并行执行策略 | 🔒 用户沉淀 |
| 流程经验 | `wf/` | 功能开发、Bug修复、重构、Git操作 | 🔒 用户沉淀 |
| 案例经验 | `cs/` | 依赖冲突、TS类型断言陷阱 | 🔒 用户沉淀 |
| 反模式警告 | `ap/` | 上下文爆炸、过度编辑、跳步验证、反复重试 | 🔒 用户沉淀 |

### Agent 角色（agents/）

| 文档 | 说明 | 状态 |
|------|------|------|
| [experience-miner.md](./agents/experience-miner.md) | 经验矿工 — 从对话中提取并沉淀经验 | ✅ 完成 |

### 部署指南（setup/）

| 文档 | 说明 | 状态 |
|------|------|------|
| [opencode.md](./setup/opencode.md) | OpenCode 安装配置指南 | ✅ 完成 |

---

## 快速索引

### 核心概念

| 概念 | 说明 |
|------|------|
| **四层架构** | 全局基座 → 经验索引 → 领域经验 → 经验提取 |
| **SKILL.md** | 经验的载体，按需加载，不污染默认上下文 |
| **经验状态** | draft → verified / failed |
| **半自动沉淀** | 任务完成/用户满意时主动询问是否提取 |
| **失败驱动验证** | 用户感知失败而非成功，失败即验证数据来源 |

### 快捷命令

| 命令 | 用途 |
|------|------|
| `/exp` | 浏览经验库索引 |
| `/mine` | 从当前对话提取经验 |

---

## 修订记录

| 版本 | 日期 | 修订内容 |
|------|------|----------|
| v0.3 | 2026-04-07 | 删除成功计数验证，失败是唯一验证信号 |
| v0.2 | 2026-04-07 | 清空预置经验内容，保留入口框架；用户通过 /mine 自行沉淀 |
| v0.1 | 2026-04-07 | 初始版本，完整四层架构设计 |
