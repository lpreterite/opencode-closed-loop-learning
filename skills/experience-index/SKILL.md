---
name: experience-index
description: 经验路由索引 — 六类知识分类模型，根据三维定位匹配经验
metadata:
  category: index
  status: core
---

# 经验路由索引

## 使用方式

这是经验系统的入口。加载后，根据当前任务在三维空间中的位置匹配经验。

### 三维定位法

判断任务在三轴上的位置，加载最相关的经验：

| 轴 | 正端 | 负端 | 判断问题 |
|----|------|------|---------|
| A | 抽象 | 具体 | 需要抽象判断还是具体操作？ |
| B | 专业 | 通用 | 跨项目通用还是本系统特有？ |
| C | 主观 | 现实 | 涉及个人偏好还是客观约束？ |

## 方法论经验 (method/)

> 通用可迁移的做事方法，跨项目可用。来源：案例+领域知识→归纳

| 经验 | 推荐加载 | 状态 |
|------|---------|------|
| Bug 修复 | method/bugfix-flow | 用户报告错误/异常 | ✅核心 |
| 新功能开发 | method/feature-dev | 用户要求添加新功能 | ✅核心 |
| 代码重构 | method/refactor-safe | 用户要求改善代码结构 | ⚡待验证 |
| Git 操作 | method/git-commit | 涉及 commit/PR 操作 | ✅核心 |
| 文件写入验证 | method/write-verify | write 工具创建新文件后 | ⚡待验证 |
| 架构不一致分析 | method/architecture-mismatch-analysis | 文档与代码不一致 | ⚡待验证 |
| 多 Subagent 协作 | method/multi-subagent-collaboration | Tester 发现 Bug 转交 Developer | ⚡待验证 |
| 代码搜索/定位 | method/search-first | 需要找文件或代码位置 | ✅核心 |
| 多文件并行操作 | method/parallel-strategy | 需要同时处理多个文件 | ✅核心 |
| 设计验证降级 | method/design-verification-fallback | 图像工具不可用 | ✅已验证 |
| 文件编辑/修改 | method/edit-over-write | 需要修改现有代码 | ⚡待验证 |
| GitHub SSH 切换 | method/https-to-ssh-fallback | git push 认证失败 | ⚡待验证 |
| 项目重命名搜索 | method/search-after-rename | 项目重命名后全局搜索 | ⚡待验证 |

## 领域工作流 (dw/)

> 绑定特定系统/业务的可执行步骤。来源：案例+领域知识→归纳

| 经验 | 推荐加载 | 状态 |
|------|---------|------|
| 分析交接补位 | dw/analysis-handoff-fallback | 子智能体完成分析但未落档 | ⚡待验证 |
| 设计稿产品验收 | dw/design-product-acceptance | 按 PRD/用户旅程验收设计稿 | ⚡待验证 |
| 周任务验收同步 | dw/weekly-task-acceptance-sync | 周目录新增 review 后同步 | ⚡待验证 |
| GitHub CI 调试 | dw/gh-ci-debug | 需要调试 GitHub Actions 失败 | ⚡待验证 |
| opencode MCP 调试 | dw/opencode-mcp-debug | mcp list 失败 / MCP 连接问题 | ⚡待验证 |
| Pencil MCP 工作流 | dw/pencil-mcp-workflow | 使用 Pencil VS Code 扩展 | ⚡待验证 |

## 案例经验 (cs/)

> 从经历中提炼的特定场景事件。来源：经历的捕获（DAG 唯一起点）

| 案例 | 推荐加载 | 信号 | 状态 |
|------|---------|------|------|
| 依赖版本冲突 | cs/dep-version-conflict | module not found / 版本报错 | ⚡待验证 |
| Go install 二进制名 | cs/go-install-binary-name | go install 后 command not found | ⚡待验证 |
| TS 类型断言陷阱 | cs/ts-type-assertion-trap | 编译通过但运行报错 | ✅已验证 |
| Pencil flexbox 修复 | cs/pencil-flexbox-fix | flexbox 布局中子节点位置偏离 | ⚡待验证 |
| Pencil 图标替换 | cs/pencil-icon-repair | 空图标需替换为 Lucide | ✅已验证 |
| CI 测试超时 | cs/ci-test-timeout | CI 失败显示超时 | ⚡待验证 |
| Golang 类型断言 Panic | cs/golang-type-assertion-panic | HTTP 连接崩溃 / connection reset | ⚡待验证 |
| Service start 短路 | cs/service-start-short-circuit | macOS service start 无输出 | ⚡待验证 |
| Homebrew API 误用 | cs/homebrew-write-api-misuse | brew install 报错 invalid option | ⚡待验证 |
| opencode annotations null | cs/opencode-annotations-null | mcp list 显示 "Failed to get tools" | ⚡待验证 |
| MCP npx broken pipe | cs/mcp-npx-broken-pipe | npx 启动 MCP 后立即断开 | ⚡待验证 |
| MCP Initialize 握手 | cs/mcp-initialize-handshake | tools/list 无响应 | ⚡待验证 |
| Go subprocess 启动延迟 | cs/go-subprocess-stdin-delay | subprocess 启动后立即发数据丢失 | ⚡待验证 |
| Pencil 属性校验错误 | cs/pencil-content-validation-error | text content 校验失败 | ⚡待验证 |

## 偏好 (pf/)

> 个人倾向的稳定表达——"队长是什么样的人"。来源：案例→反思感受

| 偏好 | 说明 | 状态 |
|------|------|------|
| _（暂无沉淀）_ | 使用 `/mine` 从对话中提取 | — |

## 领域知识 (dk/)

> 已确认的稳定约束——"这个世界有什么限制"。来源：案例→确认因果

| 知识 | 说明 | 状态 |
|------|------|------|
| _（暂无沉淀）_ | 使用 `/mine` 从对话中提取 | — |

## 反模式警告 (ap/)

> 多次失败的蒸馏红线——"绝对不能这样做"。来源：≥2 不同场景失败

| 反模式 | 推荐加载 | 信号 | 状态 |
|--------|---------|------|------|
| 上下文爆炸 | ap/context-explosion | 一次读取超过 10 个文件 | ✅核心 |
| 过度编辑 | ap/overedit | 单次修改超过 5 个文件 | ✅核心 |
| 跳步验证 | ap/skip-verify | 修改完未运行 lint/test | ✅核心 |
| 反复认证重试 | ap/repeated-auth-retry | 认证失败后持续重试 | ⚡待验证 |
| SSH 失败立即切换 | ap/ssh-retry-before-fallback | SSH 连接失败不重试直接切 HTTPS | ⚡待验证 |
| 混合范围提交 | ap/mixed-scope-commit | 提交混入非指定目录改动 | ⚡待验证 |
| 不存在的 API 参数 | ap/false-api-parameter | 使用文档不存在的参数 | ⚡待验证 |
| 文档与实现不一致 | ap/doc-implementation-mismatch | 文档定义与代码不符 | ⚡待验证 |

## 知识蒸馏 DAG

```
经历(L0)
  ↓ 捕获
案例(L1)           ← cs/ → 不问用户直接记录
  ↓
├── 反思感受 → 偏好(L2)         ← pf/ → 第1次问用户，之后自动
└── 确认因果 → 领域知识(L2)      ← dk/ → 第2次问用户，第3次晋升
       ↓ 归纳
       ├── 方法论(L3)           ← method/ → 跨域第2次问用户
       ├── 领域工作流(L3)       ← dw/ → 第3次同类问用户
       └── 反模式(L3)           ← ap/ → 第2次不同场景问用户
```

## 加载策略

1. 先加载 experience-index 获取全景
2. 根据三维定位，加载 1-2 条最相关的具体经验
3. 不要一次加载超过 3 条经验（节省上下文）
4. 经验间交叉时：优先加载 dw/（域特定）而非 method/（通用）
