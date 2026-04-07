# OpenCode 安装指南

> Setup Guide for OpenCode

**所属目录**：`opencode-closed-loop-learning/setup/`
**文档状态**：已发布
**当前版本**：v0.1
**发布日期**：2026-04-07

---

## 1. 概述

本指南说明如何将闭环学习系统部署到 [OpenCode](https://opencode.ai) 中。

---

## 2. OpenCode 闭环学习特性

| 特性 | 说明 |
|------|------|
| 指令文件 | `AGENTS.md` |
| 全局级位置 | `~/.config/opencode/AGENTS.md` |
| 项目级位置 | `./AGENTS.md` |
| 外部文件引用 | `opencode.json` instructions 字段 |
| Agent 定义 | `~/.config/opencode/agents/*.md` |
| 命令配置 | `opencode.json` command 字段 |
| Skills | `~/.config/opencode/skills/*/SKILL.md` |

---

## 3. 部署步骤

### 步骤 1：部署 AGENTS.md

将 `AGENTS.md` 复制到全局配置目录：

```bash
cp AGENTS.md ~/.config/opencode/AGENTS.md
```

### 步骤 2：部署经验库

```bash
mkdir -p ~/.config/opencode/skills
cp -r skills/* ~/.config/opencode/skills/
```

### 步骤 3：部署 Agent

```bash
cp agents/experience-miner.md ~/.config/opencode/agents/
```

### 步骤 4：配置命令

在 `~/.config/opencode/opencode.json` 中添加 command 配置：

```bash
# 编辑 ~/.config/opencode/opencode.json，在适当位置添加：
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

### 步骤 5：验证部署

重启 OpenCode 后执行：

```
/exp        # 应显示经验索引
/mine       # 应启动经验提取流程
```

---

## 4. 目录结构

部署后，`~/.config/opencode/` 应包含：

```
~/.config/opencode/
├── opencode.json                          # 已追加 command 配置
├── AGENTS.md                              # 全局基座规则
│
├── agents/
│   └── experience-miner.md                # 经验矿工 Agent
│
└── skills/
    ├── experience-index/SKILL.md
    ├── tp/search-first/SKILL.md
    ├── tp/edit-over-write/SKILL.md
    ├── tp/parallel-strategy/SKILL.md
    ├── wf/feature-dev/SKILL.md
    ├── wf/bugfix-flow/SKILL.md
    ├── wf/refactor-safe/SKILL.md
    ├── wf/git-commit/SKILL.md
    ├── cs/dep-version-conflict/SKILL.md
    ├── cs/ts-type-assertion-trap/SKILL.md
    ├── ap/context-explosion/SKILL.md
    ├── ap/overedit/SKILL.md
    └── ap/skip-verify/SKILL.md
```

---

## 5. 验证清单

```
□ ~/.config/opencode/AGENTS.md 存在
□ ~/.config/opencode/skills/experience-index/SKILL.md 存在
□ ~/.config/opencode/skills/tp/search-first/SKILL.md 存在
□ ~/.config/opencode/skills/wf/feature-dev/SKILL.md 存在
□ ~/.config/opencode/skills/ap/context-explosion/SKILL.md 存在
□ ~/.config/opencode/agents/experience-miner.md 存在
□ opencode.json 包含 /mine 和 /exp 命令
```
