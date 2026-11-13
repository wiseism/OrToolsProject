# 修复记录：CI Python 3.9 在 ubuntu-26.04 runner 上不可用（2026-11-08）

## 失败信息
- 仓库：wiseism/OrToolsProject
- Workflow：PuLP Optimization CI
- 失败 run：[42157396504](https://github.com/wiseism/OrToolsProject/actions/runs/42157396504)（2026-11-06）

## 错误日志摘录
```
##[error]The version '3.9' with architecture 'x64' was not found for Ubuntu 26.04.
```
（windows 3.9/3.10 两个 job 因 matrix 失败被连带 canceled）

## 原因分析
ubuntu-latest 已迁移到 Ubuntu 26.04，runner 镜像不再内置 CPython 3.9；
setup-python@v4 也无法从源码/缓存取到该版本，matrix 组合 `ubuntu-latest + 3.9` 必失败。

## 修复内容
`.github/workflows/pulp-ci.yml`（提交 71eaa6d，已在远程）：
- 与 `ubuntu-latest` 组合的 `3.9` 加入 `strategy.matrix.exclude`。

## 验证
| run id | 结果 |
| --- | --- |
| 42590376904 | ✅ success |
