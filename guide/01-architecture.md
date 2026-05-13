# 系统架构

> System Architecture

**文档状态**：已发布
**当前版本**：v0.2
**最后更新**：2026-05-13

---

## 1. 设计目标

在 OpenCode 中构建一套**按需加载经验的闭环学习系统**，使 AI 编程助手能够：

1. **按需加载**：根据当前任务类型，自动匹配并加载相关经验
2. **经验分类**：六类知识模型 + 工具优先级，共七类经验
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
| Layer 2 | `skills/tp/ method/ dw/ cs/ pf/ dk/ ap/` | 根据索引推荐 | 每条约 200-500 token |
| Layer 3 | `agents/experience-miner.md` | 半自动触发 | 仅提取时占用 |

### 2.2 渐进式披露协议

四层之间按**渐进式披露**策略组织信息，不把下层内容全部挤入上层上下文：

```
Layer 0 (instructions / AGENTS.md)    ← 始终加载
│
├── 身份信号        "帕奇喜欢简洁直接"
├── 底线规则        "不在 raw 层改写原文"
├── 信号→技能映射    "如果发现自己在做 X → 加载 pf/xxx"
│                   "如果检测到模式 Y → 加载 ap/yyy"
│
Layer 1 (experience-index)            ← 按需加载
│   经验全景索引，帮助 Agent 判断"当前任务该加载什么"
│
Layer 2 (SKILL.md 文件集)              ← 按需展开
│   完整的经验内容 + 边界说明 + 历史案例
│   skill() 加载时不塞入上下文，用完 compaction 自动回收
│
Layer 3 (experience-miner)            ← 仅提取时加载
    经验提取与修正 Agent
```

**披露规则**：

| 信息类型 | 放置位置 | 理由 |
|---------|---------|------|
| 身份信号、底线规则 | L0 instructions | 必须始终生效，不能依赖按需加载 |
| 信号→技能映射 | L0 instructions | 让 Agent 明确知道"什么情况该加载什么 skill" |
| 经验索引全景 | L1 experience-index | 需要时再加载，节省上下文 |
| 完整经验内容 | L2 SKILL.md | 篇幅最大，用时才展开 |
| 提取流程 | L3 agent | 仅在沉淀经验时使用 |

**为什么偏好和反模式不能完全放在 L2？**

偏好和反模式是身份/底线级别的信息——偏好定义"队长是什么样的人"，反模式定义"绝对不能做的事"。如果只放在 L2，依赖 Agent 自行判断何时加载，存在遗漏风险。L0 放核心信号 + 映射规则，既保证底线不漏，又不把全文塞进上下文。

---

## 3. 闭环流程

```
                    ┌──────────────────┐
                    │   用户发起任务    │
                    └────────┬─────────┘
                             ▼
                    ┌──────────────────┐
      Layer 0 ──→   │  全局基座自动加载  │  ← AGENTS.md + instructions（身份信号 + 底线规则 + 映射）
                    └────────┬─────────┘
                             ▼
                    ┌──────────────────┐
     Layer 1 ──→   │  加载经验索引     │  ← skill: experience-index
                    │  匹配当前任务     │
                    └────────┬─────────┘
                             ▼
                    ┌──────────────────┐
      Layer 2 ──→   │  按需加载领域经验  │  ← skill: tp/ method/ dw/ cs/ pf/ dk/ ap
                    │  指导任务执行     │
                    └────────┬─────────┘
                             ▼
                    ┌──────────────────┐
                    │   执行任务        │
                    └────────┬─────────┘
                             ▼
                     ┌──────────────────┐
      半自动   ──→   │  用户纠正 AI     │  ← 失败是唯一验证信号
                     │  触发 /mine      │
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
                     │  状态标记         │  ← draft → verified/failed
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
| 方法论 | method/ | 通用可迁移的做事方法 | skills/method/ |
| 领域工作流 | dw/ | 绑定特定系统/业务的可执行步骤 | skills/dw/ |
| 案例经验 | cs/ | 从经历中提炼的特定场景事件 | skills/cs/ |
| 偏好 | pf/ | 个人倾向的稳定表达 | skills/pf/ |
| 领域知识 | dk/ | 已确认的稳定约束 | skills/dk/ |
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
        ├── tp/                               # 工具优先级经验
        │   ├── search-first/
        │   ├── edit-over-write/
        │   └── ...
        │
        ├── method/                           # 方法论（通用可迁移）
        │   ├── bugfix-flow/
        │   ├── feature-dev/
        │   ├── git-commit/
        │   └── ...
        │
        ├── dw/                               # 领域工作流（绑定特定系统）
        │   ├── analysis-handoff-fallback/
        │   ├── design-product-acceptance/
        │   └── ...
        │
        ├── cs/                               # 案例经验
        │   ├── ts-type-assertion-trap/
        │   ├── dep-version-conflict/
        │   └── ...
        │
        ├── pf/                               # 偏好（个人倾向）
        │   └── SKILL.md (模板)
        │
        ├── dk/                               # 领域知识（稳定约束）
        │   └── SKILL.md (模板)
        │
        └── ap/                               # 反模式警告
            ├── context-explosion/
            ├── overedit/
            ├── skip-verify/
            └── ...
```

---

## 7. 加载时机详解

| 场景 | 三维定位 | 推荐加载 |
|------|---------|---------|
| 代码搜索/定位 | 具体+通用+中性 | tp/search-first |
| Bug 修复 | 偏抽象+通用+偏现实 | method/bugfix-flow |
| TS 类型断言陷阱 | 具体+中性+中间 | cs/ts-type-assertion-trap |
| 上下文爆炸 | 最抽象+偏通用+偏主观 | ap/context-explosion |
| 分析交接补位 | 偏抽象+专业+偏现实 | dw/analysis-handoff-fallback |
| 用户表达偏好 | 偏具体+通用+最主观 | pf/ （新建偏好） |
| 需要确认约束 | 中间+专业+偏现实 | dk/ （新建知识） |

> 三维定位轴：A 抽象↔具体 / B 通用↔专业 / C 主观↔现实
