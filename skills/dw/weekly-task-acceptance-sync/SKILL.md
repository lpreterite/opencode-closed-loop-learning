---
name: weekly-task-acceptance-sync
description: 周任务目录中完成专项验收后，按 review、problem、STATUS、tasklist 四类文档同步落档
metadata:
  category: domain-workflow
  status: unverified
  created: "2026-04-09"
  verified_count: 0
---

# 每周任务目录的验收落档同步流程

## 第一步：确认周目录与落档位置

1. 定位对应周目录，如 `docs/project-tasks/W14/`
2. 核对该周已有的 `STATUS.md`、`tasklist.md`、`problems/` 结构
3. 若是专项验收，优先使用具名文件名，如 `review-j1-create-script-ui-acceptance.md`

## 第二步：先写专项 review 文档

review 文档至少包含：

1. 验收对象与范围
2. 验收依据
3. 页面/状态覆盖表
4. 问题分级与结论
5. 整改优先级

命名上优先体现线路或专题，而不是只写通用 `review.md`。

## 第三步：把关键问题拆成独立 problem 单

1. 在 `problems/` 下按编号新增问题文件
2. 文件名格式建议为：`NNN-专题-问题摘要.md`
3. review 中每个 P0/P1 问题都尽量回链到独立问题单

## 第四步：同步更新周状态文档

在 `STATUS.md` 中至少补充：

1. 已完成的专项验收记录
2. 已新增的问题单列表
3. 当前结论（通过 / 未通过 / 待修复）
4. 阻塞项或残余风险的更新

## 第五步：同步更新 tasklist

1. 在对应任务或验收标准下补记专项验收结果
2. 若验收未通过，要把阻塞问题写回任务条目，而不是只写在 review 里
3. 若阶段任务已完成专项验收，可在验收产出区标注已落档的 review/problem 编号

## 第六步：检查四类文档是否互相可追踪

最少要形成以下链路：

`tasklist.md` → `review-*.md` → `problems/*.md`

并且 `STATUS.md` 中能够回看本次验收的结论和问题编号。

## 关键原则

- 专项验收不要只留在聊天记录，必须落到周目录中
- review 负责“结论汇总”，problem 负责“问题单独跟踪”，两者不要混用
- `STATUS.md` 反映周状态，`tasklist.md` 反映任务状态，二者都要同步
- 命名和编号要稳定，便于后续 agent 继续追踪整改

## 边界与不适用场景

- 不适用于临时讨论纪要；无正式验收结论时不必新建 review
- 不适用于全周总验收归档；那通常使用统一 `review.md`
- 若本周目录尚未建立，应先按项目规范补齐基础结构
