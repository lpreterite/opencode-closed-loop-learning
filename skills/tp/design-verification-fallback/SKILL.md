---
name: design-verification-fallback
description: 图像识别工具不可用时，通过结构化数据验证设计修复的替代策略
metadata:
  category: tool-priority
  status: verified
  created: "2026-04-07"
  verified_count: 1
---

# 设计验证的降级策略

## 核心问题

设计修改后的视觉验证依赖图像识别工具（MiniMax_understand_image 等），但这些工具可能：
1. 服务不可用（返回 "Not connected"）
2. 模型不支持图像输入
3. 超时或返回错误

## 降级验证方案

### 级别 1：结构验证（pencil_batch_get）✅ 始终可用

通过读取节点树验证：
- 子节点数量是否正确
- 节点类型（type）是否正确
- 关键属性值（fill、width、height、iconFontName）是否符合预期
- 旧节点是否已删除
- 新节点是否在正确位置

```
验证清单：
□ 子节点数量 = 预期数量
□ 每个子节点的 type 正确
□ 关键属性值匹配设计规格
□ 没有残留的旧节点 ID
□ 节点顺序正确
```

### 级别 2：布局验证（pencil_snapshot_layout）✅ 始终可用

通过坐标数据验证：
- 节点位置（x, y）是否符合预期
- 节点尺寸（width, height）是否正确
- 居中计算：x = (parentWidth - nodeWidth) / 2
- clipped 状态（不应被裁剪）
- 子节点在父容器内居中

```
居中验证公式：
容器内居中：childX = (containerWidth - childWidth) / 2
  例：24px 图标在 56px 容器中 → x = (56-24)/2 = 16 ✓
组件内居中：containerX = (componentWidth - containerWidth) / 2
  例：56px 容器在 240px 组件中 → x = (240-56)/2 = 92 ✓
```

### 级别 3：图像验证（MiniMax / screenshot）⚠️ 可能不可用

理想情况但非必需：
- pencil_export_nodes 导出 PNG
- MiniMax_understand_image 分析图像
- pencil_get_screenshot 获取预览

## 经验教训

1. **先做结构验证再做图像验证** — 结构正确是视觉正确的前提
2. **将图像验证视为锦上添花** — 结构+布局数据足以确认 90% 的修复
3. **告知用户手动验证** — 当图像工具不可用时，导出截图让用户自行查看
4. **导出截图仍然有价值** — 即使 AI 无法分析，用户可以手动打开文件

## 典型输出格式

验证完成时，输出结构化的验证表格：

```
| 验证项 | 结果 |
|--------|------|
| 图标容器尺寸 | 56×56 ✅ |
| 图标居中 | x:16,y:16 ✅ |
| 图标内容 | lucide:file-text ✅ |
| 旧节点清除 | 已删除 ✅ |
```

这种表格让用户一目了然地确认修复是否符合预期，即使没有截图也能建立信心。
