---
name: pencil-icon-repair
description: Pencil 设计中替换空图标为 Lucide icon_font 的标准操作案例
metadata:
  category: case-study
  status: verified
  created: "2026-04-07"
  verified_count: 1
---

# Pencil 图标替换案例

## 问题场景

空内容组件中的图标是灰色空圆形，缺少实际图标内容。需要替换为带 Lucide 图标的正确设计。

## 根因

组件内使用空的 frame 节点模拟图标，缺少 `icon_font` 类型的实际图标内容。

## 修复模式

### 标准操作序列（4 步）

```
1. D("旧容器ID")           — 删除包含空图标的容器（子节点一并删除）
2. I("父组件", 新图标容器)  — 插入新的图标容器 frame
3. I(新容器, icon_font节点) — 在容器内插入 Lucide 图标
4. M(新容器, "父组件", 0)  — 移到组件首位
```

### 关键参数

**图标容器（frame）**：
- 尺寸：56×56
- cornerRadius：28（圆形）
- fill：#F0F0F0（浅灰背景）
- layout："vertical"
- justifyContent："center"
- alignItems："center"

**图标内容（icon_font）**：
- 尺寸：24×24（在 56px 容器中居中）
- iconFontFamily："lucide"
- fill：#999999（灰色图标）

### 常用 Lucide 图标映射

| 场景 | iconFontName |
|------|-------------|
| 空文档/无内容 | file-text |
| 搜索无结果 | search-x |
| 空文件夹 | folder-open |
| 网络错误 | wifi-off |
| 无数据 | database |

## 注意事项

- 删除容器前确认 ID 正确（用 pencil_batch_get 预读验证）
- icon_font 的 fill 是图标颜色，不是容器背景色
- 一次 batch_design 调用中 D/I/M 可以链式执行，binding 在同一次调用内可用
- 不同 batch_design 调用之间 binding 不可复用

## 验证方法

用 pencil_batch_get 检查最终结构：
- 确认 icon_font 节点存在且属性正确
- 确认没有残留的旧节点
- 确认子节点顺序正确（图标在最前）

用 pencil_snapshot_layout 检查布局：
- 图标在容器内 x:16, y:16（24px 在 56px 中居中）
- 图标在父组件内水平居中（x = (width - 56) / 2）
