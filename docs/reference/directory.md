# 项目文档目录结构规范

> Directory Structure Convention — 遵循 AI 软件研发工程体系（ai-engineering）规范

**所属目录**：`docs/reference/`
**文档状态**：已发布
**当前版本**：v0.1
**发布日期**：2026-05-24
**来源仓库**：`lpreterite/ai-engineering`
**源文件路径**：`reference/directory.md`

---

## 1. 标准目录结构

```
opencode-closed-loop-learning/
├── setup.md                       # ★ Agent 执行入口
├── README.md                      # 项目概述
├── skills/                        # 经验库（OpenCode 技能系统）
│   ├── experience-index/
│   ├── method/
│   ├── dw/
│   ├── cs/
│   ├── pf/
│   ├── dk/
│   └── ap/
│
└── docs/                          # 文档根目录
    ├── STATUS.md                  # 项目状态卡（PM Agent 核心输入/输出）
    ├── README.md                  # 文档索引
    ├── guide/                     # 设计文档
    ├── agents/                    # Agent 角色定义
    ├── setup/                     # 工具安装指南
    └── reference/                 # 参考资料
```

---

## 2. 目录用途说明

| 目录/文件 | 用途 | 维护者 |
|-----------|------|--------|
| `setup.md` | Agent 执行入口：部署规范、角色一览、工具配置 | PM Agent |
| `README.md` | 项目概述：系统原理、经验分类、文件结构 | 项目维护者 |
| `skills/` | 经验库（OpenCode Skills 系统实际加载路径） | 经验矿工 Agent |
| `docs/` | 所有文档的根目录 | PM Agent |
| `docs/STATUS.md` | 项目状态卡：当前阶段、里程碑、阻塞项 | PM Agent |
| `docs/README.md` | 文档索引，指向所有关键文档 | PM Agent |
| `docs/guide/` | 设计文档：系统架构、使用指南、经验管理等 | 项目维护者 |
| `docs/agents/` | Agent 角色定义：经验矿工等 | 项目维护者 |
| `docs/setup/` | 工具安装指南：OpenCode 部署 | 项目维护者 |
| `docs/reference/` | 参考资料：目录结构规范、外部方法论 | 项目维护者 |

---

## 3. 命名规范

| 规则 | 示例 |
|------|------|
| 目录使用 kebab-case | `docs/reference/`, `docs/setup/` |
| 文件使用 kebab-case | `experience-management.md`, `directory.md` |
| 大写用于特殊文件 | `STATUS.md`, `README.md`, `SKILL.md` |

---

## 4. 维护规则

| 事件 | 操作 | 执行者 |
|------|------|--------|
| 项目初始化 | 创建完整 `docs/` 目录结构 | PM Agent |
| 添加新指南 | 在 `docs/guide/` 下按编号命名 | 项目维护者 |
| 添加新 Agent | 在 `docs/agents/` 下创建角色文件 | 项目维护者 |
| 里程碑完成 | 更新 `docs/STATUS.md` | PM Agent |

---

## 5. 相关文档

| 文档 | 路径 |
|------|------|
| AI 软件研发工程体系 | [ai-engineering](https://github.com/lpreterite/ai-engineering) |
| Repo 目录初始化指南 | [07-repo-directory-guide.md](https://github.com/lpreterite/ai-engineering/blob/main/guide/07-repo-directory-guide.md) |
| 项目状态卡 | [../STATUS.md](../STATUS.md) |
| 文档索引 | [../README.md](../README.md) |
