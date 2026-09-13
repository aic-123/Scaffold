---
id: "con-9009"
type: 概念
title: "一个自己给自己发认证的节点"
aliases: []
cues: ["随便什么处境"]
scope: "演示 R7。不适用于任何真实场景。"
source:
  ref: "https://example.invalid/self-certified"
  kind: 个人
evidence_status: 已验证
relations: []
filled_by: 人
notes: "故意留错：演示 R7"
---

演示 **R7 状态越权自声明**：`evidence_status` 被手写成了「已验证」。
这是本项目最重的一条 ERROR。

`已验证` 不是一种"我觉得对"的状态，是一个**授权**——
它只能由验证方通过 hooks 契约写入（见 `hooks/validator-interface.md`）。

**注意：这个节点即使补了出处也没用。** `source.ref` 有值、`kind` 是「个人」，
但 `已验证` 依然报错。因为出处解决的是"这话从哪来"，而 `已验证` 回答的是"**有人验过了**"——
这两件事不能互相顶替。这是整个契约里唯一的权限声明。

> 把 `filled_by` 改成 `模型(...)`，校验器还会额外点明"**疑似模型代填**"。
> 那是本项目立项根基被踩中的信号。
