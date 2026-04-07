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

| 类别 | 状态 | 数量 |
|------|------|------|
| 经验索引 | ✅ 完成 | 1条 |
| 工具优先级 | 🔒 预留 | 7条 |
| 流程经验 | 🔒 预留 | 4条 |
| 案例经验 | 🔒 预留 | 4条 |
| 反模式警告 | 🔒 预留 | 4条 |

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

### 工具优先级经验 (tp/)

| 经验 | 状态 | 说明 |
|------|------|------|
| (7条) | 🔒 | 本地经验，详见 ~/.config/opencode/skills/tp/ |

### 流程经验 (wf/)

| 经验 | 状态 | 说明 |
|------|------|------|
| (4条) | 🔒 | 本地经验，详见 ~/.config/opencode/skills/wf/ |

### 案例经验 (cs/)

| 经验 | 状态 | 说明 |
|------|------|------|
| (4条) | 🔒 | 本地经验，详见 ~/.config/opencode/skills/cs/ |

### 反模式警告 (ap/)

| 经验 | 状态 | 说明 |
|------|------|------|
| (4条) | 🔒 | 本地经验，详见 ~/.config/opencode/skills/ap/ |

---

## 里程碑

- [x] 完成系统架构设计（四层模型）
- [x] 创建目录结构
- [x] 创建 README.md 和 STATUS.md
- [x] 创建设计文档（4篇 guide）
- [x] 部署初始经验库（15条）
- [x] 清理 symlink，替换为实际内容文件
- [x] 清空经验内容，保留目录结构（本地经验不暴露）
- [x] 创建经验矿工 Agent
- [x] 创建 OpenCode 部署指南
- [x] Git 仓库初始化
- [x] 首次 commit（v0.1）
- [x] v0.2 推送（失败驱动修正验证机制）

---

## 阻塞项

暂无

---

## 最近更新

```
2026-04-07 v0.2：验证机制升级为"失败驱动修正"，新增 2 条经验（https-to-ssh-fallback, repeated-auth-retry），清理 symlink，清空经验内容
2026-04-07 v0.1：初始版本，完整四层架构设计 + 13条种子经验 + 经验矿工 Agent
```
