# samples-broken · 校验器的靶子

这个目录里的节点**都是故意写错的**。它们唯一的作用，是演示校验器能抓什么。

```bash
python validator/validate.py ./samples-broken
```

跑一遍：**10 个节点，ERROR 8 / WARN 1，退出码 `1`，R1–R8 八条规则全部命中。**

| 文件 | 演示的规则 | 级别 |
|---|---|---|
| `r1-orphan.md` | R1 孤立节点 | ERROR |
| `r2-hypothesis-no-source.md` | R2 缺出处 | ERROR |
| `r3-cycle-a.md` + `r3-cycle-b.md` | R3 循环依赖 | ERROR |
| `r4-judge-no-vars.md` | R4 判断点没声明变量 | ERROR |
| `r4-stance-no-issue.md` | R4 立场没有议题可站 | ERROR |
| `r5-prefix-mismatch.md` | R5 id 前缀与类型不符 | ERROR |
| `r6-empty-cues.md` | R6 情境索引缺失 | **WARN** |
| `r7-self-verified.md` | R7 状态越权自声明 | ERROR |
| `r8-filler-claims-empty.md` | R8 声明未填充却有内容 | ERROR |

> R3 需要两个节点才能成环，所以占了两个文件。其余规则各一个。

每个文件的正文里都写明了"错在哪、为什么算错、怎么修"。

**这个目录与 `samples/` 是两个独立的世界：**

- `samples/` 是**形状示范**——干净通过（退出码 `0`），留白处只产生 WARN
- `samples-broken/` 是**规则示范**——故意留错（退出码 `1`）

两者不要放在一起校验：id 段不同（`samples/` 用 `0001`–`0199`，这里用 `9001`–`9099`），
但混跑会让报告变难读。

> 这些节点**不是知识**，是校验器的测试用例。`filled_by` 多数写 `模型(示例数据)`，
> 但有**两个刻意的例外**，各自是为了走一条特定的分支：
>
> | 文件 | `filled_by` | 为什么 |
> |---|---|---|
> | `r7-self-verified.md` | `人` | R7 有两条分支：`filled_by` 是模型时报"**疑似模型代填**"，否则报通用的"不可手工写入"。用 `人` 是为了走后者（手写场景）。想看前者，改成 `模型(...)` 即可 |
> | `r8-filler-claims-empty.md` | `未填充` | 它演示的**就是**这个取值与内容矛盾。改成 `人` 或 `模型(...)`，R8 立刻消失 |

<!-- Copyright 2026 ks-demo · SPDX-License-Identifier: Apache-2.0 -->
