<!-- Copyright 2026 AIC-123 · SPDX-License-Identifier: Apache-2.0 -->

## 这次提交说了几件事？

（一次提交说一件事——本项目的版本审计全靠 diff 读得懂）

## 实跑输出（必填，不要只描述）

```
$ python validator/validate.py --self-test
（粘贴）

$ python validator/validate.py ./samples
（粘贴，应退出码 0）

$ python validator/validate.py ./samples-broken
（粘贴，应退出码 1）
```

## 自查（逐条确认）

- [ ] 改了数据 → 已 grep 文档的**输出块 / 计数 / 示例名**三样
- [ ] 没有提交行尾变化（仓库是 LF，`.gitattributes` 已钉死）
- [ ] 没有提交真实领域内容（上游不收真实内容——收录本身就是一次权威背书）
- [ ] 若新增规则：已先开 issue 并取得设计方认账

## 若改了 schema

- [ ] 同步改了 `validator/validate.py` 里的镜像常量
- [ ] `--self-test` 通过（它专门查这个一致性）
