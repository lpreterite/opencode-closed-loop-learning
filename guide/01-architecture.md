# 系统架构

> System Architecture

**文档状态**：已发布
**当前版本**：v0.1
**最后更新**：2026-04-07

---

## 1. 设计目标

在 OpenCode 中构建一套**按需加载经验的闭环学习系统**，使 AI 编程助手能够：

1. **按需加载**：根据当前任务类型，自动匹配并加载相关经验
2. **经验分类**：工具优先级、流程、案例、反模式四类经验
3. **闭环沉淀**：从对话中提取有价值经验，持续积累
4. **质量验证**：新经验需经过验证后才标记为可靠

---

## 2. 四层架构

```
┌─────────────────────────────────────────────────────┐
│  Layer 0: 全局基座 (AGENTS.md + instructions)        │  ← 会话启动时自动加载
├─────────────────────────────────────────────────────┤
│  Layer 1: 经验索引 (skill: experience-index)          │  ← 按需加载，路由决策
├─────────────────────────────────────────────────────┤
│  Layer 2: 领域经验库 (skills: tp/ wf/ cs/ ap/)        │  ← 按需加载，具体经验
├─────────────────────────────────────────────────────┤
│  Layer 3: 经验提取 Agent (agent: experience-miner)    │  ← 对话后半自动触发
└─────────────────────────────────────────────────────┘
```

### 2.1 各层职责

| 层级 | 文件位置 | 加载时机 | 上下文占用 |
|------|---------|---------|-----------|
| Layer 0 | `AGENTS.md` + `instructions` | 每次会话启动 | 固定，约 500 token |
| Layer 1 | `skills/experience-index/` | Agent 判断需要时 | 约 300 token |
| Layer 2 | `skills/tp/ wf/ cs/ ap/` | 根据索引推荐 | 每条约 200-500 token |
| Layer 3 | `agents/experience-miner.md` | 半自动触发 | 仅提取时占用 |

---

## 3. 闭环流程

```
                    ┌──────────────────┐
                    │   用户发起任务    │
                    └────────┬─────────┘
                             ▼
                    ┌──────────────────┐
     Layer 0 ──→   │  全局基座自动加载  │  ← AGENTS.md + instructions
                    └────────┬─────────┘
                             ▼
                    ┌──────────────────┐
     Layer 1 ──→   │  加载经验索引     │  ← skill: experience-index
                    │  匹配当前任务     │
                    └────────┬─────────┘
                             ▼
                    ┌──────────────────┐
     Layer 2 ──→   │  按需加载领域经验  │  ← skill: tp/ wf/ cs/ ap
                    │  指导任务执行     │
                    └────────┬─────────┘
                             ▼
                    ┌──────────────────┐
                    │   执行任务        │
                    └────────┬─────────┘
                             ▼
                    ┌──────────────────┐
     半自动   ──→   │  Agent 检测经验   │  ← 任务完成/用户满意时主动询问
                    │  询问是否提取     │
                    └────────┬─────────┘
                             ▼
                    ┌──────────────────┐
     Layer 3 ──→   │  /mine 提取经验   │  ← agent: experience-miner
                    │  沉淀到经验库     │
                    └────────┬─────────┘
                             ▼
                    ┌──────────────────┐
                    │  经验库更新       │  ← SKILL.md 文件更新
                    │  索引更新         │  ← experience-index 更新
                    │  状态标记         │  ← unverified → verified
                    └────────┬─────────┘
                             ▼
                    ┌──────────────────┐
                    │  下次会话可用 ✅   │
                    └──────────────────┘
```

---

## 4. OpenCode 特性映射

本系统充分利用 OpenCode 的以下原生特性：

| OpenCode 特性 | 本系统用途 | 文件位置 |
|---------------|-----------|---------|
| **instructions** | 全局中文指令，会话启动时加载 | `opencode.json → instructions` |
| **AGENTS.md (Rules)** | 全局基座规则，经验加载协议 | `~/.config/opencode/AGENTS.md` |
| **Skills (SKILL.md)** | 经验的主要载体，按需加载 | `~/.config/opencode/skills/*/SKILL.md` |
| **Agents (Markdown)** | 经验提取专用 Agent | `~/.config/opencode/agents/experience-miner.md` |
| **Commands** | `/mine` 和 `/exp` 快捷命令 | `opencode.json → command` |
| **Compaction** | 自动清理已加载经验，节省上下文 | 自动工作，无需配置 |
| **Permissions** | 控制哪些 Agent 可以修改经验库 | `opencode.json → permission` |

---

## 5. 经验分类体系

| 类别 | 前缀 | 说明 | 目录 |
|------|------|------|------|
| 工具优先级 | tp/ | 工具使用的优先级和组合策略 | skills/tp/ |
| 流程经验 | wf/ | 完成某类任务的标准化步骤 | skills/wf/ |
| 案例经验 | cs/ | 解决具体问题的完整案例 | skills/cs/ |
| 反模式 | ap/ | 应该避免的做法和信号 | skills/ap/ |

---

## 6. 目录结构

```
~/.config/opencode/
├── opencode.json                          # 主配置（instructions + commands）
├── AGENTS.md                              # Layer 0: 全局基座规则
├── zh-cn-instructions.md                  # 现有中文指令（保留不动）
│
├── agents/
│   └── experience-miner.md                # Layer 3: 经验提取 Agent
│
└── skills/
    ├── experience-index/                  # Layer 1: 经验路由索引
    │   └── SKILL.md
    │
    ├── tp/                                # 工具优先级经验
    │   ├── search-first/
    │   ├── edit-over-write/
    │   └── parallel-strategy/
    │
    ├── wf/                                # 流程经验
    │   ├── feature-dev/
    │   ├── bugfix-flow/
    │   ├── refactor-safe/
    │   └── git-commit/
    │
    ├── cs/                                # 案例经验
    │   ├── dep-version-conflict/
    │   └── ts-type-assertion-trap/
    │
    └── ap/                                # 反模式警告
        ├── context-explosion/
        ├── overedit/
        └── skip-verify/
```

---

## 7. 加载时机详解

| 场景 | 推荐加载 | 触发条件 |
|------|---------|---------|
| 代码搜索/定位 | tp/search-first | 需要找文件或代码位置 |
| 文件编辑/修改 | tp/edit-over-write | 需要修改文件 |
| 多文件并行操作 | tp/parallel-strategy | 需要同时操作多个文件 |
| 新功能开发 | wf/feature-dev | 用户要求添加新功能 |
| Bug 修复 | wf/bugfix-flow | 用户报告错误/异常 |
| 代码重构 | wf/refactor-safe | 用户要求改善代码结构 |
| Git 操作 | wf/git-commit | 涉及 commit/PR 操作 |
| 依赖版本冲突 | cs/dep-version-conflict | module not found / 版本报错 |
| TS 类型断言陷阱 | cs/ts-type-assertion-trap | 编译通过但运行报错 |
| 上下文爆炸 | ap/context-explosion | 一次读取超过 10 个文件 |
| 过度编辑 | ap/overedit | 单次修改超过 5 个文件 |
| 跳步验证 | ap/skip-verify | 修改完未运行 lint/test |
