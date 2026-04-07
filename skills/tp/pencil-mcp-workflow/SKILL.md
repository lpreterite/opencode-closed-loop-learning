---
name: pencil-mcp-workflow
description: Pencil VS Code 扩展的截图、导出、布局验证的正确流程
metadata:
  category: tool-priority
  status: unverified
  created: "2026-04-06"
  verified_count: 0
---

# Pencil MCP 工具使用优先级

## 工具流程

```
pencil_snapshot_layout → 获取布局数据，发现问题
        ↓
pencil_get_screenshot → 获取截图预览（返回图像但无法直接查看）
        ↓
pencil_export_nodes → 导出为 PNG 文件到本地
        ↓
MiniMax_understand_image → 用 MCP 工具分析图像
```

## 优先级排序

1. **pencil_snapshot_layout** — 首选，获取布局快照
   - 返回节点布局信息（x, y, width, height, clipped 状态）
   - 用于发现定位问题、裁剪问题
   - 设置 maxDepth 控制遍历深度

2. **pencil_batch_design** — 执行批量修改
   - U() 更新节点属性
   - I() 插入新节点
   - R() 替换节点
   - C() 复制节点

3. **pencil_get_screenshot** — 获取视觉预览
   - 返回图像数据但模型无法直接查看
   - 仅确认图像生成成功

4. **pencil_export_nodes** — 导出为文件
   - 导出到 /tmp 或项目 tmp 目录
   - 需要手动创建目标目录
   - 返回文件路径供后续分析

5. **pencil_batch_get** — 读取节点数据
   - 按 ID 或模式搜索节点
   - 设置 resolveInstances 展开组件实例

## 典型场景

### 场景 1：验证修复结果
```
pencil_snapshot_layout(parentId="组件ID")
→ 检查 x/y 是否正确、clipped 状态
→ 如需视觉确认：
pencil_get_screenshot(nodeId="组件ID")
pencil_export_nodes(nodeIds=["组件ID"], outputDir="/tmp/pencil-export")
→ MiniMax_understand_image 分析
```

### 场景 2：查看设计系统组件
```
pencil_batch_get(patterns=[{reusable: true}], searchDepth=3)
→ 列出所有可复用组件
pencil_batch_get(nodeIds=["组件ID"], resolveInstances=true)
→ 展开组件内部结构
```

## 关键发现

- `pencil_get_screenshot` 返回图像数据但**不自动保存到文件**
- 需要 `pencil_export_nodes` 手动导出
- 小文件用 MiniMax_understand_image 分析，**大文件可能超时**
- OCR 提取文字可用 `zai-mcp-server_extract_text_from_screenshot`

## 反模式

- 不要依赖 `pencil_get_screenshot` 直接查看图像（无法在当前架构查看）
- 不要跳过 `pencil_snapshot_layout` 直接修改（没有目标）
- 大幅导出时设置合理的 scale 参数避免超时
