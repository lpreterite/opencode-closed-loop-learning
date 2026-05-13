---
name: homebrew-write-api-misuse
description: Homebrew Formula 中使用不存在的 backup 参数导致 install 失败
metadata:
  category: case-study
  status: unverified
  created: "2026-04-19"
  verified_count: 0
---

# 案例：Homebrew .write() API 误用

## 症状

```bash
$ brew install ./mcp-gateway.rb
Error: invalid option: `--backup'
# 或
Error: undefined method 'backup=' for #<Formula::Writer:...>
```

## 定位过程

### 1. 错误观察
- Formula 安装失败
- 错误信息指向 `backup: true` 选项

### 2. 检查 Formula 代码
```ruby
# 错误代码
def install
  # ...
  # 错误地使用了不存在的 backup 选项
  inreplace "config.yaml", "old_value", "new_value", backup: true
end
```

### 3. 查阅 Homebrew 文档
- `inreplace` 方法**不支持** `backup:` 参数
- 该参数是误用，实际上应该用其他方式处理

## 根因

对 Homebrew API 的误解和文档误读：

1. **`inreplace` 方法签名**：
   ```ruby
   inreplace(path, before, after, missing_ok: false)
   ```
   不存在 `backup` 参数

2. **混淆了不同工具的 API**：
   - 可能混淆了 `sed -i` 的 `-i.bak` 备份语义
   - 或混淆了 Ruby 的 `File.write` 选项

## 解决方案

改用条件判断模式处理配置文件存在性：

```ruby
def install
  # 使用 unless + exist? 判断，避免需要 backup 选项
  unless (config_file = buildpath/"config.yaml").exist?
    # 生成默认配置文件
  end
  
  # inreplace 不使用 backup 参数
  inreplace "config.yaml", "old_value", "new_value"
  
  # 或者完全不同的方式：直接写入完整文件
end
```

## 教训

- **使用 API 前必读文档**：不要假设参数存在，查阅官方文档
- **陌生 API 要验证**：对于不熟悉的 gem/API，先写测试验证
- **错误信息是线索**：`invalid option` 或 `undefined method` 直接指向了问题所在
