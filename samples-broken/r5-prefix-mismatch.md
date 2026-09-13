---
id: "arg-9007"
type: 概念
title: "一个 id 前缀与类型不符的节点"
aliases: []
cues: ["随便什么处境"]
scope: "演示 R5。不适用于任何真实场景。"
source:
  ref: ""
  kind: 未知
evidence_status: 未验证
relations: []
filled_by: 模型(示例数据)
notes: "故意留错：演示 R5"
---

演示 **R5 id 冲突或格式错**：`type` 是「概念」，id 却写成 `arg-9007`（`arg` 是论据的前缀）。

id 是这个仓库里唯一的硬链接。前缀与类型对不上，`relations` 就全成了猜谜。

校验器会报 R5。修法：把 id 改成 `con-9007`，或者把 `type` 改成「论据」。

> R5 还管另外三件事：id 格式不是「前缀-四位序号」、同一个 id 出现两次、
> 以及 `relations` 指向一个不存在的 id（悬挂引用）。
