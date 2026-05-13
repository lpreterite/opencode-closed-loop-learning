---
name: dep-version-conflict
description: 依赖版本冲突的定位和解决案例
metadata:
  category: case-study
  status: unverified
  created: "2026-04-06"
  verified_count: 0
---

# 案例：依赖版本冲突

## 症状
- 构建失败
- 错误信息提到 "module not found" 或 "version mismatch"
- 运行时报错 "Cannot find module"

## 定位过程

1. `cat package.json | grep <module>` → 确认依赖声明
2. `ls node_modules/<module>` → 确认是否安装
3. `npm ls <module>` → 查看依赖树，找到版本冲突
4. 检查 lock 文件中的版本锁定情况

## 根因
peer dependency 版本不匹配，或 lock 文件与 package.json 不同步。

## 解决方案

1. 锁定版本号到兼容范围
2. `rm -rf node_modules package-lock.json && npm install`（彻底重装）
3. 如使用 workspace，检查 workspace 协议是否正确

## 教训
遇到 "module not found" 先查依赖树再删重装，不要盲目猜测。
