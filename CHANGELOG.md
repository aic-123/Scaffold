# 变更日志

<!-- Copyright 2026 AIC-123 · SPDX-License-Identifier: Apache-2.0 -->

本项目遵循 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)。
版本号采用 `v0.x.y`——设计稿 v0 阶段，结构可能随时变动。

## [Unreleased]

## [v0.0.1] - 2026-09-14

首个公开版本。tag 于 2026-09-13 建立（当时仓库私有、无人可下载），2026-09-14 首次公开，
并同步了改名与显示名。**所以这一版以"公开时的状态"为准**——这样拿到 tag 的人，
README 里的 clone 地址与仓库名是对得上的。

### 包含

- 节点结构规格（`SPEC.md`）+ 机器可读镜像（`schema/node.schema.yaml`）
- 只读校验器 `validator/validate.py`：规则 R1–R8 + `--todo` 待填清单 + `--self-test`
- 干净样本 `samples/`（23 节点，退出码 0）与规则靶子 `samples-broken/`（10 节点，退出码 1）
- CI（`.github/workflows/ci.yml`）：Python 3.9 与 3.13 双分支，含"靶子必须命中"的反转断言
- 验证钩子接口契约 `hooks/validator-interface.md`（不绑定任何服务）
- 贡献指南 `CONTRIBUTING.md`

### 修正

- 仓库改名 `ks-demo` → `Scaffold`，同步三处地址：`README.md` 与 `README.en.md` 的 clone 命令、
  `CONTRIBUTING.md` 的 issue 入口（共 3 文件 5 处）
- 项目显示名改为 `Scaffold`：只动两份 README 的标题（`README.md:3`、`README.en.md:3`）。
  正文里"知识结构工具"作为**泛指品类**的用法（`README.md:59`、`hooks/validator-interface.md:14`）
  以及 `validator/validate.py` 的模块说明**保持不变**——它们说的是"它是什么"，不是"它叫什么"
- CI 增加 `workflow_dispatch`，可在 Actions 页手动触发——改仓库设置、改可见性这类操作不会触发 `push`

### 已知未覆盖

- 检索、内容库、版本迁移、**多语言标签**、权限、并发、验证方对接
- 本结构在真实、大量、有分歧的知识上是否够用，**尚未被检验过**

### 未决

- **R8 待设计方认账**（见 `validator/rules.md` 末节）
- **④⑤⑥ 层的 `type` 归属**是执行时推断的，设计稿只给了层名与内容
