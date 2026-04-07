# OpenCode 闭环学习系统｜状态卡

> **Last Updated**：2026-04-07
> **Index**：[README](./README.md)

---

## 概览

**OpenCode 闭环学习系统** — 实现 AI 编程助手的按需加载经验和半自动沉淀

**当前阶段**：设计完成，初始经验库已部署

---

## 文档进度

### 设计文档（guide/）

| 文档 | 状态 | 路径 |
|------|------|------|
| 系统架构 | ✅ 完成 | [guide/01-architecture.md](./guide/01-architecture.md) |
| 日常使用指南 | ✅ 完成 | [guide/02-usage.md](./guide/02-usage.md) |
| 经验管理 | ✅ 完成 | [guide/03-experience-management.md](./guide/03-experience-management.md) |
| 部署指南 | ✅ 完成 | [guide/04-deployment.md](./guide/04-deployment.md) |

### 经验库（skills/）

| 类别 | 状态 | 说明 |
|------|------|------|
| 经验索引 | ✅ 完成 | 入口框架，用户通过 /mine 自行沉淀 |
| 工具优先级 | 🔒 预留 | tp/ 目录，用户沉淀 |
| 流程经验 | 🔒 预留 | wf/ 目录，用户沉淀 |
| 案例经验 | 🔒 预留 | cs/ 目录，用户沉淀 |
| 反模式警告 | 🔒 预留 | ap/ 目录，用户沉淀 |

### Agent 角色（agents/）

| 文档 | 状态 | 路径 |
|------|------|------|
| 经验矿工 | ✅ 完成 | [agents/experience-miner.md](./agents/experience-miner.md) |

### 部署指南（setup/）

| 文档 | 状态 | 路径 |
|------|------|------|
| OpenCode 安装配置 | ✅ 完成 | [setup/opencode.md](./setup/opencode.md) |

---

## 经验库清单

> 经验库为空，用户通过 `/mine` 命令从对话中沉淀经验。

| 类别 | 目录 | 状态 |
|------|------|------|
| 工具优先级 | `tp/` | 🔒 待沉淀 |
| 流程经验 | `wf/` | 🔒 待沉淀 |
| 案例经验 | `cs/` | 🔒 待沉淀 |
| 反模式警告 | `ap/` | 🔒 待沉淀 |

---

## 里程碑

- [x] 完成系统架构设计（四层模型）
- [x] 创建目录结构
- [x] 创建 README.md 和 STATUS.md
- [x] 创建设计文档（4篇 guide）
- [x] 创建经验矿工 Agent
- [x] 创建 OpenCode 部署指南
- [x] Git 仓库初始化
- [x] v0.1 首次 commit
- [x] 清空预置经验，保留入口框架
- [x] v0.2 推送

---

## 阻塞项

暂无

---

## 最近更新

```
2026-04-07 v0.3：删除成功计数验证，失败是唯一验证信号
2026-04-07 v0.2：清空预置经验，保留入口框架，用户通过 /mine 自行沉淀
2026-04-07 v0.1：初始版本，完整四层架构设计 + 经验矿工 Agent
```
