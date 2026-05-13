# OpenCode 闭环学习系统｜状态卡

> **Last Updated**：2026-05-13
> **Index**：[README](./README.md)

---

## 概览

**OpenCode 闭环学习系统** — 实现 AI 编程助手的按需加载经验和半自动沉淀

**当前阶段**：v2 — 六类知识分类模型升级完成，经验库已对齐

---

## 文档进度

### 设计文档（guide/）

| 文档 | 状态 | 路径 |
|------|------|------|
| 系统架构 | ✅ 完成 | [guide/01-architecture.md](./guide/01-architecture.md) |
| 日常使用指南 | ✅ 完成 | [guide/02-usage.md](./guide/02-usage.md) |
| 经验管理 | ✅ 完成 | [guide/03-experience-management.md](./guide/03-experience-management.md) |
| 部署指南 | ✅ 完成 | [guide/04-deployment.md](./guide/04-deployment.md) |
| 六类知识模型改版方案 | ✅ 完成 | [guide/05-revamp.md](./guide/05-revamp.md) |

### 经验库（skills/）— v2 六类知识模型

| 类别 | 前缀 | 状态 | 说明 |
|------|------|------|------|
| 经验索引 | experience-index | ✅ 完成 | 六类索引 + 三维定位 + DAG 蒸馏 |
| 方法论 | method/ | ✅ 完成 | 13 条经验（含从 tp/ 合并的 6 条通用策略） |
| 领域工作流 | dw/ | ✅ 完成 | 6 条经验（含从 tp/ 合并的 3 条系统绑定经验） |
| 领域工作流 | dw/ | ✅ 完成 | 3 条经验，从 wf/ 迁移 |
| 案例经验 | cs/ | ✅ 完成 | 14 条经验 |
| 偏好 | pf/ | 🔶 框架 + 模板 | 模板就位，内容待沉淀 |
| 领域知识 | dk/ | 🔶 框架 + 模板 | 模板就位，内容待沉淀 |
| 反模式 | ap/ | ✅ 完成 | 8 条经验 |

### Agent 角色（agents/）

| 文档 | 状态 | 路径 |
|------|------|------|
| 经验矿工 | ✅ 完成 | [agents/experience-miner.md](./agents/experience-miner.md) |

### 部署指南（setup/）

| 文档 | 状态 | 路径 |
|------|------|------|
| OpenCode 安装配置 | ✅ 完成 | [setup/opencode.md](./setup/opencode.md) |

---

## 经验库

> 按六类知识分类模型组织，经验内容为个人本地沉淀，不在状态卡中详列。

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
- [x] 预置 4 条通用经验示例
- [x] v0.3：删除成功计数验证
- [x] README 人性化重写（学徒与师傅）
- [x] v0.4：六类知识模型升级 — wf/ → method/ + dw/，新增 pf/ + dk/
- [x] v0.5：tp/ 归入 method/ + dw/，六类知识模型最终对齐

---

## 阻塞项

暂无

---

## 最近更新

```
2026-05-13 v0.5：tp/ 归入 method/ + dw/，六类知识模型最终对齐
2026-05-13 v0.4：六类知识模型升级 — wf/ 拆分为 method/ + dw/，新增 pf/ + dk/
2026-04-07 v0.3：删除成功计数验证，失败是唯一验证信号
2026-04-07 v0.2：清空预置经验，保留入口框架，用户通过 /mine 自行沉淀
2026-04-07 v0.1：初始版本，完整四层架构设计 + 经验矿工 Agent
```
