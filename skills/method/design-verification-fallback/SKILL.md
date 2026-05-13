---
name: design-verification-fallback
description: 图像识别工具不可用时，通过结构化数据验证设计修复的替代策略
metadata:
  category: methodology
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

### 级别 3：图像验证（MCP → ui-check 本地兜底）⚠️ 可能不可用

理想情况但非必需，按优先级递减：

#### L3-1：MCP 图像工具（优先使用）

- `pencil_export_nodes` 导出 PNG
- `gateway_zhipu_understand_image` / `MiniMax_understand_image` 分析图像
- `pencil_get_screenshot` 获取预览

#### L3-2：ui-check 本地工具（降级兜底）✅ MCP 不可用时使用

当 MCP 图像工具不可用（Not connected / 超时 / 模型不支持图像）时，使用本地 `ui-check` CLI 工具：

**前提条件**：
- 已安装 `ui-check`（`go install github.com/lpreterite/ui-check/cmd/cli@latest`）
- 本地运行 Ollama 服务（`ollama serve`）
- 已拉取模型（`ollama pull qwen3.5:2b`）

**使用方式**：

```bash
# 单图验证
ui-check verify screenshot.png -i "描述预期设计"

# 多图对比
ui-check verify before.png after.png -i "对比修复前后差异"

# 指定模型
ui-check verify screenshot.png -i "描述" --model qwen3.5:2b

# 显示详细日志
ui-check verify screenshot.png -i "描述" -v
```

**输出解析**：

```
[qwen3.5:2b]
[地址] http://localhost:11434
[图片] screenshot.png

---
这里是模型返回的原始分析内容...
---
```

**验证策略**：
1. 导出修复后的截图：`pencil_export_nodes`
2. 使用 `ui-check verify` 分析截图是否符合设计描述
3. 提取输出中的关键判断（匹配/不匹配/问题点）
4. 如果 ui-check 也失败，降级到纯结构+布局数据验证

**典型指令模板**：

| 验证场景 | ui-check 指令 |
|---------|--------------|
| 组件存在性 | `ui-check verify img.png -i "图片中央有一个红色圆形图标"` |
| 布局位置 | `ui-check verify img.png -i "图标位于页面右上角"` |
| 颜色验证 | `ui-check verify img.png -i "背景色为蓝色 (#1E88E5)"` |
| 修复对比 | `ui-check verify before.png after.png -i "从旧版到新版，图标已从左侧移到右侧"` |

## 经验教训

1. **先做结构验证再做图像验证** — 结构正确是视觉正确的前提
2. **图像验证降级顺序：MCP → ui-check 本地 → 手动查看** — ui-check 是可靠的本地兜底方案
3. **将图像验证视为锦上添花** — 结构+布局数据足以确认 90% 的修复
4. **告知用户手动验证** — 当所有工具不可用时，导出截图让用户自行查看
5. **导出截图仍然有价值** — 即使 AI 无法分析，用户可以手动打开文件

## 典型输出格式

验证完成时，输出结构化的验证表格：

```
| 验证项 | 结果 |
|--------|------|
| 图标容器尺寸 | 56×56 ✅ |
| 图标居中 | x:16,y:16 ✅ |
| 图标内容 | lucide:file-text ✅ |
| 旧节点清除 | 已删除 ✅ |
| 图像 AI 验证 | ✅ 通过（ui-check）|
```

这种表格让用户一目了然地确认修复是否符合预期，即使没有截图也能建立信心。

### ui-check 验证表示例

当使用 ui-check 进行图像验证时：

```
| 验证项 | MCP 工具 | ui-check 兜底 |
|--------|---------|---------------|
| 图像识别 | ❌ Not connected | ✅ 可用 |
| 验证结果 | - | "图标位于右上角，与预期一致" ✅ |
| 可信度 | - | 高（本地模型，无需网络）|
```

**降级流程图**：

```
pencil_export_nodes (导出截图)
         ↓
  MCP 图像工具可用？
    ├─ 是 → MiniMax_understand_image → 输出分析
    └─ 否 → ui-check verify → 输出分析
              ↓
         ui-check 也失败？
           ├─ 是 → 纯结构+布局验证，告知用户手动查看
           └─ 否 → 完成验证
```
