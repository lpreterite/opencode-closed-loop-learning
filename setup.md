# OpenCode 闭环学习系统 — Agent 执行入口

> Agent Execution Entry Point
> 遵循 AI 软件研发工程体系（ai-engineering）规范

**所属目录**：`opencode-closed-loop-learning/`
**文档状态**：已发布
**当前版本**：v0.1
**发布日期**：2026-05-24
**来源仓库**：`lpreterite/ai-engineering`
**源文件路径**：`setup.md`

---

## 1. 概述

本仓库是 OpenCode 闭环学习系统的实现仓库。本文档作为 **Agent 执行入口**，在 Agent 接手任务时提供部署规范、角色一览和工具配置。

---

## 2. 项目信息

| 属性 | 值 |
|------|-----|
| **项目名称** | OpenCode 闭环学习系统 |
| **作用** | 实现 AI 编程助手的按需加载经验和半自动沉淀 |
| **当前状态** | v0.5 — 六类知识模型最终对齐 |
| **状态卡** | [docs/STATUS.md](./docs/STATUS.md) |
| **文档索引** | [docs/README.md](./docs/README.md) |

---

## 3. 仓库结构

```
opencode-closed-loop-learning/
├── setup.md              # ★ 本文件（Agent 执行入口）
├── README.md             # 项目概述
├── skills/               # 经验库（OpenCode 实际加载）
│
└── docs/                 # 文档根目录
    ├── STATUS.md         # 项目状态卡
    ├── README.md         # 文档索引
    ├── guide/            # 设计文档（架构/用法/经验管理/部署/改版）
    ├── agents/           # Agent 角色定义
    ├── setup/            # 工具安装指南
    └── reference/        # 参考资料
```

---

## 4. 角色一览

| 角色 | 类型 | 核心职责 | 文档 |
|------|------|----------|------|
| **经验矿工** | Agent | 从失败修正中提取经验并优化经验库 | [docs/agents/experience-miner.md](./docs/agents/experience-miner.md) |

---

## 5. 配置参考

| 配置项 | 位置 | 说明 |
|--------|------|------|
| 全局基座规则 | `~/.config/opencode/AGENTS.md` | 经验加载协议 + DAG 蒸馏规则 |
| 全局中文指令 | `~/.config/opencode/zh-cn-instructions.md` | 中文友好指令 |
| 经验库 | `~/.config/opencode/skills/*/SKILL.md` | 六类知识模型 |
| 命令配置 | `~/.config/opencode/opencode.json → command` | `/mine` + `/exp` |

---

## 6. 快速索引

| 入口 | 路径 |
|------|------|
| 项目状态 | [docs/STATUS.md](./docs/STATUS.md) |
| 文档索引 | [docs/README.md](./docs/README.md) |
| 系统架构 | [docs/guide/01-architecture.md](./docs/guide/01-architecture.md) |
| 经验矿工 | [docs/agents/experience-miner.md](./docs/agents/experience-miner.md) |
| 部署指南 | [docs/setup/opencode.md](./docs/setup/opencode.md) |
| 目录规范 | [docs/reference/directory.md](./docs/reference/directory.md) |
