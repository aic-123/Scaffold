---
id: "arg-9003"
type: 论据
title: "A 要靠 B 才成立"
aliases: []
cues: ["随便什么处境"]
scope: "演示 R3。不适用于任何真实场景。"
source:
  ref: ""
  kind: 未知
evidence_status: 未验证
relations: ["arg-9004"]
filled_by: 模型(示例数据)
notes: "故意留错：演示 R3"
---

演示 **R3 循环依赖**：A 指向 B，B 又指回 A。

环上的节点互为前提——往下读会无限循环，往上追溯也没有一个落脚点。

校验器会报 R3，并给出完整环路。修法：删掉环上任意一条 `relations`。

> 这类环最常出现在 `立场 ↔ 论据` 这种天然互引的关系上。
> 所以 `relations` 约定为**单向写**，反方向用正文文字引用。见 `SPEC.md`。
