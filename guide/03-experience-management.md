# 经验管理

> Experience Management

**文档状态**：已发布
**当前版本**：v0.1
**最后更新**：2026-04-07

---

## 1. 经验状态流转

```
  [新提取]                [被成功应用1次]          [被应用3+次]
  unverified ──────────→ verified ──────────→ core
  ⚡待验证                ✅已验证(N)            🏆核心
```

### 状态定义

| 状态 | 标记 | 含义 | verified_count |
|------|------|------|---------------|
| unverified | ⚡待验证 | 新提取，未经实践验证 | 0 |
| verified | ✅已验证(N) | 已被 N 次任务成功应用 | 1-2 |
| core | 🏆核心 | 多次验证的可靠经验 | 3+ |

---

## 2. 质量验证机制

### 2.1 验证触发

当一条 `unverified` 经验被加载并在任务中成功应用时：

1. Agent 在任务完成后识别"本次使用了哪条经验"
2. 下次 `/mine` 时更新该经验的 `verified_count`
3. 达到阈值自动升级状态
4. 更新 `experience-index` 中的状态标记

### 2.2 升级规则

| 条件 | 状态变化 |
|------|---------|
| 创建新经验 | → unverified |
| 被成功应用 1-2 次 | → verified |
| 被成功应用 3+ 次 | → core |

---

## 3. 经验提取流程

### 3.1 经验识别

扫描对话历史，识别以下类型的经验：

| 类型 | 分类 | 信号 |
|------|------|------|
| 更优的工具使用顺序或组合 | tp/ | 发现了更高效的工具组合 |
| 复杂任务的标准化步骤 | wf/ | 完成了某个可复制的流程 |
| 具体问题的解决方案 | cs/ | 解决了某个具体的 bug |
| 应该避免的做法 | ap/ | 发现了某种反模式 |

### 3.2 经验去重

读取 `experience-index`，检查是否已有高度相似的经验：

- 如已有：考虑补充到现有经验中而非新建
- 如没有：准备创建新经验

### 3.3 经验命名

为新经验确定一个简洁的英文短名称：

- 格式：`小写-短横线-分隔`
- 长度：2-4 个单词
- 示例：`parallel-grep`、`env-var-check`、`promise-error-silent`

### 3.4 文件创建

按照对应类型的模板创建 SKILL.md 文件。

### 3.5 索引更新

在 `experience-index` 的对应分类表中追加新条目。

---

## 4. 经验清理机制

定期（每月或经验超过 50 条时）：

- 删除长期 unverified 且从未被加载的经验
- 合并高度相似的 verified 经验
- 归档过时的经验（标记为 deprecated）

---

## 5. SKILL.md 文件格式

每条经验都是独立的 `SKILL.md` 文件，包含以下元数据：

```yaml
---
name: <经验名称>
description: <一句话描述>
metadata:
  category: <分类：tool-priority/workflow/case-study/anti-pattern>
  status: <状态：unverified/verified/core>
  created: "<创建日期>"
  verified_count: <验证次数>
---
```

### 模板参考

详见 `agents/experience-miner.md` 中的完整模板说明。
