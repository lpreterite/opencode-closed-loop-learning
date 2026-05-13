# 部署指南

> Deployment Guide

**文档状态**：已发布
**当前版本**：v0.1
**最后更新**：2026-04-07

---

## 1. 概述

本指南说明如何将闭环学习系统部署到 OpenCode 中。

**部署位置**：`~/.config/opencode/`

---

## 2. 部署步骤

### 步骤 1：创建目录结构

```bash
mkdir -p ~/.config/opencode/skills/{experience-index,tp, wf/{feature-dev,bugfix-flow,refactor-safe,git-commit},cs/{dep-version-conflict,ts-type-assertion-trap},ap/{context-explosion,overedit,skip-verify}}
mkdir -p ~/.config/opencode/agents
```

### 步骤 2：部署配置文件

| 源文件 | 目标位置 | 说明 |
|--------|---------|------|
| `AGENTS.md` | `~/.config/opencode/AGENTS.md` | 全局基座规则 |
| `skills/*/SKILL.md` | `~/.config/opencode/skills/*/` | 经验库 |
| `agents/experience-miner.md` | `~/.config/opencode/agents/` | 经验矿工 Agent |

### 步骤 3：配置命令

在 `~/.config/opencode/opencode.json` 中添加命令配置：

```json
{
  "command": {
    "mine": {
      "template": "请分析当前对话，提取其中有价值的经验（工具使用技巧、流程经验、案例教训、反模式），并将它们以独立 SKILL.md 文件的形式沉淀到 ~/.config/opencode/skills/ 对应分类目录中。使用 @经验矿工 智能体执行。完成后更新 experience-index。",
      "description": "从当前对话中提取经验到经验库",
      "agent": "build"
    },
    "exp": {
      "template": "加载经验索引 skill({ name: \"experience-index\" })，展示当前可用的所有经验类别和条目，包括状态标注（✅核心/⚡待验证）。如用户指定了具体经验名，直接加载该经验。",
      "description": "浏览经验库索引",
      "agent": "build"
    }
  }
}
```

---

## 3. 配置文件说明

### 3.1 AGENTS.md

全局基座规则，包含：
- 经验加载协议（何时加载什么经验）
- 经验沉淀判断规则（何时询问用户提取）

### 3.2 experience-index/SKILL.md

经验路由索引，根据当前任务类型推荐下一步加载的经验。

### 3.3 命令配置

| 命令 | 功能 |
|------|------|
| `/mine` | 提取当前对话经验到经验库 |
| `/exp` | 浏览经验库索引 |

---

## 4. 验证部署

检查以下文件都存在：

```bash
ls ~/.config/opencode/AGENTS.md
ls ~/.config/opencode/skills/experience-index/SKILL.md
ls ~/.config/opencode/skills/tp/search-first/SKILL.md
ls ~/.config/opencode/skills/method/bugfix-flow/SKILL.md
ls ~/.config/opencode/skills/ap/context-explosion/SKILL.md
ls ~/.config/opencode/agents/experience-miner.md
```

验证命令是否可用：

```
/exp        # 应显示经验索引
/mine       # 应启动经验提取流程
```

---

## 5. 目录结构总览

```
~/.config/opencode/
├── opencode.json                          # 主配置（追加 command）
├── AGENTS.md                              # 全局基座规则
│
├── agents/
│   └── experience-miner.md                # 经验矿工 Agent
│
    └── skills/
        ├── experience-index/
        │   └── SKILL.md                      # 经验路由索引
        ├── tp/                               # 工具优先级
        │   ├── search-first/SKILL.md
        │   ├── edit-over-write/SKILL.md
        │   └── parallel-strategy/SKILL.md
        ├── method/                           # 方法论（通用可迁移）
        │   ├── bugfix-flow/SKILL.md
        │   ├── feature-dev/SKILL.md
        │   ├── refactor-safe/SKILL.md
        │   ├── git-commit/SKILL.md
        │   ├── write-verify/SKILL.md
        │   └── architecture-mismatch-analysis/SKILL.md
        ├── dw/                               # 领域工作流（绑定特定系统）
        │   ├── analysis-handoff-fallback/SKILL.md
        │   ├── design-product-acceptance/SKILL.md
        │   └── weekly-task-acceptance-sync/SKILL.md
        ├── cs/                               # 案例经验
        │   ├── dep-version-conflict/SKILL.md
        │   ├── ts-type-assertion-trap/SKILL.md
        │   └── ...
        ├── pf/                               # 偏好（新建）
        │   └── SKILL.md (模板)
        ├── dk/                               # 领域知识（新建）
        │   └── SKILL.md (模板)
        └── ap/                               # 反模式
            ├── context-explosion/SKILL.md
            ├── overedit/SKILL.md
            └── skip-verify/SKILL.md
```

---

## 6. 扩展方向

| 方向 | 说明 |
|------|------|
| 项目级经验 | 在 `.opencode/skills/` 中存放项目特定的经验 |
| 团队共享 | 通过 Git 仓库共享经验库，团队协同积累 |
| 经验评分 | 记录每条经验的"帮助度"评分（由用户反馈） |
| MCP 集成 | 通过 MCP Server 实现跨工具的经验检索 |
| 自动化验证 | 通过 Custom Tool 自动检测经验是否被成功应用 |
