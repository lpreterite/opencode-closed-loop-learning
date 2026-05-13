---
name: false-api-parameter
description: 使用不存在的 API 参数导致命令失败
metadata:
  category: anti-pattern
  status: unverified
  created: "2026-04-19"
  verified_count: 0
---

# 反模式：使用不存在的 API 参数

## 识别信号

当出现以下错误信息时，可能是误用了不存在的 API 参数：

```
Error: invalid option: '--xxx'
Error: undefined method 'xxx=' for #<Class:...>
Error: wrong number of arguments (given X, expected Y)
```

## 常见场景

### 1. 混淆不同工具的 API
```ruby
# 混淆了 sed -i.bak 和 Ruby inreplace
inreplace "file", "old", "new", backup: true  # ❌ 不存在

# sed -i.bak 的备份语义不应套用到 inreplace
```

### 2. 想当然地添加参数
```ruby
# 假设存在某个"显而易见"的参数
something do |option: true|  # ❌ 该选项不存在
```

### 3. 文档过时或阅读不完整
```python
# 文档说支持 xxx 参数，实际是新版本才添加
# 或者文档描述模糊，误读了含义
```

## 规避策略

### 策略 1：先验证后使用
在正式代码中使用新 API 前，先在 REPL 中验证：
```ruby
# 验证 inreplace 是否支持 backup 参数
pry> method(:inreplace).parameters
# => [[:key, :path], [:opt, :missing_ok]]
# 确认没有 :key 或 :opt 类型的 backup 参数
```

### 策略 2：查阅官方文档
- 优先阅读官方 API 文档
- 查看方法的完整签名
- 注意版本差异

### 策略 3：使用前搜索源码
```bash
# 在 gem 源码中搜索参数名
grep -r "backup" $(gem environment gemdir)/gems/homebrew-*/
```

## 正确做法

```ruby
# ❌ 错误：使用不存在的 backup 参数
inreplace "config.yaml", "old", "new", backup: true

# ✅ 正确：使用确凿存在的参数
inreplace "config.yaml", "old", "new", missing_ok: true

# ✅ 正确：需要备份时，先复制文件
cp "config.yaml", "config.yaml.bak"
inreplace "config.yaml", "old", "new"
```

## 总结

> **"如果某个参数看起来太方便了，以至于让你怀疑它是否真实存在，那它很可能不存在。"**
