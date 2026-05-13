---
name: pencil-flexbox-fix
description: 修复 Pencil flexbox 布局中子节点位置偏离和裁剪问题
metadata:
  category: case-study
  status: unverified
  created: "2026-04-06"
  verified_count: 0
---

# 案例：Pencil flexbox 布局修复

## 症状

- 子节点 x/y 坐标严重偏离（如 x=428 但组件宽度只有 200）
- 图标/元素尺寸不一致
- 字号大小不统一
- 节点显示为 "fully clipped"（被裁剪）

## 定位过程

1. `pencil_snapshot_layout` 获取布局数据
2. 检查子节点的 x/y 和父组件的 layout 属性
3. 发现问题：子节点使用绝对定位 x/y，但父组件未设置 flexbox layout

## 根因

- **错误**：在 flexbox 布局中尝试使用绝对定位 x/y
- Pencil 的 flexbox 布局会**忽略**子节点的 x/y 属性
- 父组件未设置 `layout: "vertical"` 或 `layout: "horizontal"`

## 解决方案

### 第一步：设置父组件为 Flexbox
```javascript
U("父组件ID", { layout: "vertical" })
```

### 第二步：重置子节点坐标
```javascript
// 将 x/y 设为 0 或省略，由 flexbox 自动布局
U("子节点ID", { x: 0, y: 0 })
```

### 第三步：设置对齐方式
- `alignItems: "center"` — 水平居中
- `justifyContent: "center"` — 垂直居中

### 第四步：验证
```javascript
pencil_snapshot_layout(parentId="父组件ID")
// 检查 clipped 状态应为 false
```

## 修复示例

修复 S2.7 空内容组件：
```javascript
// 1. 父组件改为 vertical flexbox
U("S2.7内容区域", { layout: "vertical" })

// 2. 子节点坐标归零
U("S2.7标题", { x: 0, y: 0 })
U("S2.7图标", { x: 0, y: 0 })
U("S2.7CTA按钮", { x: 0, y: 0 })

// 3. 设置居中
U("S2.7内容区域", { alignItems: "center", justifyContent: "center" })
```

## 教训

修复 flexbox 布局时，**先设 layout 再调子节点**，不要在 flexbox 中混用绝对定位。
