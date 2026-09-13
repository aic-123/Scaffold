---
id: "stance-9006"
type: 立场
title: "一个没有议题可站的立场"
aliases: []
cues: ["随便什么处境"]
scope: "演示 R4。不适用于任何真实场景。"
source:
  ref: ""
  kind: 未知
evidence_status: 未验证
relations: ["arg-9003"]
filled_by: 模型(示例数据)
notes: "故意留错：演示 R4"
---

演示 **R4 类型与字段不匹配**：`type` 是「立场」，但 `relations` 里没有指向任何「议题」。

立场必须有议题可站。没有议题的立场，其实是一个断言——它不需要选边，只是在陈述。

校验器会报 R4。修法：补一条指向 `issue-NNNN` 的 `relations`，或者把 `type` 改成「断言」。

> 注意这个节点的 `relations` **不是空的**（它指向一个论据），所以它不触发 R1。
> 它触发 R4 的原因是"指向的东西类型不对"，而不是"什么都没指"。
