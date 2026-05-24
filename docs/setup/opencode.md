# OpenCode 安装指南

> Setup Guide for OpenCode

**所属目录**：`opencode-closed-loop-learning/setup/`
**文档状态**：已发布
**当前版本**：v0.2
**发布日期**：2026-05-24

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
cp docs/agents/experience-miner.md ~/.config/opencode/agents/
```

### 步骤 4：配置命令

在 `~/.config/opencode/opencode.json` 中添加 command 配置：

```bash
# 编辑 ~/.config/opencode/opencode.json，在适当位置添加：
{
  "command": {
    "mine": {
      "template": "分析当前对话，按六类知识分类模型识别有价值经验，以独立 SKILL.md 文件沉淀到 ~/.config/opencode/skills/ 对应目录：方法(method/)、领域工作流(dw/)、案例(cs/)、偏好(pf/)、领域知识(dk/)、反模式(ap/)。蒸馏时机：案例直接记录，偏好第1次问用户，领域知识第2次问用户，方法论跨域同型问用户，领域工作流第3次同类问用户，反模式第2次不同场景问用户。使用 @经验矿工 执行，完成后更新 experience-index。",
      "description": "按六类知识分类模型从当前对话提取经验",
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
    ├── method/search-first/SKILL.md
    ├── method/bugfix-flow/SKILL.md (原 wf/)
    ├── dw/analysis-handoff-fallback/SKILL.md (原 wf/)
    ├── cs/ts-type-assertion-trap/SKILL.md
    ├── pf/SKILL.md (模板)
    ├── dk/SKILL.md (模板)
    └── ap/context-explosion/SKILL.md
```

> 预置 4 条通用经验示例。更多经验由用户在日常使用中通过 `/mine` 沉淀。

### 4.1 项目仓库结构

本仓库按 [ai-engineering 研发工程体系](https://github.com/lpreterite/ai-engineering) 组织文档：

```
opencode-closed-loop-learning/
├── setup.md              # ★ Agent 执行入口
├── README.md             # 项目概述
├── skills/               # 经验库（OpenCode 实际加载）
│
└── docs/                 # 文档根目录
    ├── STATUS.md         # 项目状态卡
    ├── README.md         # 文档索引
    ├── guide/            # 设计文档
    ├── agents/           # Agent 角色定义
    ├── setup/            # 工具安装指南
    └── reference/        # 参考资料
```

---

## 5. 验证清单

```
□ ~/.config/opencode/AGENTS.md 存在（v2 六类知识模型）
□ ~/.config/opencode/skills/experience-index/SKILL.md 存在
□ ~/.config/opencode/skills/method/search-first/SKILL.md 存在
□ ~/.config/opencode/skills/method/bugfix-flow/SKILL.md 存在
□ ~/.config/opencode/skills/dw/analysis-handoff-fallback/SKILL.md 存在
□ ~/.config/opencode/skills/cs/ts-type-assertion-trap/SKILL.md 存在
□ ~/.config/opencode/skills/pf/SKILL.md 存在
□ ~/.config/opencode/skills/dk/SKILL.md 存在
□ ~/.config/opencode/skills/ap/context-explosion/SKILL.md 存在
□ ~/.config/opencode/agents/experience-miner.md 存在（v2）
□ opencode.json 包含 /mine 和 /exp 命令
```
