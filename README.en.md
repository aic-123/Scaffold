[中文](README.md) | English

# Knowledge Structure Tool (demo v0)

> Most knowledge tools end up doing one thing: turning what you don't know
> into something that looks like what you do.

---

## What it is

A knowledge-node **spec that ships structure and no content**, plus a read-only validator.

- A node is a `.md` file: YAML front matter plus an optional body. `git diff`-able, hand-editable, plain text
- Every node hangs off **two indexes**: `title` (concept index) and `cues` (situation index)
- **The situation index is the real entry point.** The concept index asks "what is this called";
  the situation index asks "what situation am I in right now". The latter is the entry point,
  because experts trigger knowledge by *recognizing the situation*, not by walking a concept tree
- The validator answers "is this structurally consistent" — never "is this knowledge correct"
- Zero runtime dependencies beyond Python 3.9+ and PyYAML. No database, no graph store, no RDF, no daemon

## What it is NOT

⭐ **This section matters more than the one above.**

- **Does not generate answers.** No retrieval, no Q&A, no "recommended for your question"
- **Does not fill in content.** When a field's value is unknown, it stays empty. The entire design
  exists to protect that emptiness
- **Does not judge for you.** The validator never says "trust this one" or "this source is more
  authoritative". Authority never enters any sort order
- **Is not professional advice.** There is not a single word of real domain content in this repo
- **Is not complete.** The six layers are a capability ceiling, not a requirement
- ⚠️ **This is a feasibility demo of a structure spec, not a usable product.**
  No retrieval, no content library, no verification. It proves exactly one thing:
  that these constraints can be turned into running code

### What we do NOT constrain (no restriction — and that is written down too)

The section above lists what this tool **does not do**. This one lists what it
**does not govern**. Both need to be stated, or you will assume a rule exists and go
edit your own files to obey it.

- **How a case's body is written is not checked.**
  In [SPEC](SPEC.md), layer ⑤ (cases) reads "situation features + options considered +
  reasoning + outcome + counterfactual" — but of those five, **only situation features
  has a field to live in** (namely `cues`). The other four can only go in the body, and
  **the body is free**: how many sections, what headings, what order — all yours.
  Measured: a `案例` node with a **completely empty body** still passes validation
  (exit code `0`, zero findings). So `type: 案例` only guarantees "it calls itself a
  case" — **it does not guarantee it has the shape of one**.
- **Where a case's `relations` point, and in which direction, is not specified.**
  SPEC's direction table has only five rows, covering six types: decision points,
  conditions, exceptions, stances, arguments, concepts. Cases are not among them.
  A case pointing at anything, of any type, in any direction, is not an error.
  For a case's `relations` the validator checks exactly two things:
  **does the referenced id exist**, and **is there a cycle** (R3). Whether the
  direction is right — it cannot check, and does not intend to.

**In one sentence: there is no restriction here.** Write it your way and nothing will
complain — the price is that you get no guarantee either: the validator will not tell
you whether your case is complete.

> Both of these are **deliberate, not unfinished**. Adding a rule would force people to
> retrofit their own files just to pass validation — and the case layer is optional
> expressive power to begin with (the six layers are a ceiling, not a requirement).

### ⚠️ On "AI filling in content" — this is the project's founding principle, not a feature trade-off

**If a model fills in content on a human's behalf, this project has no reason to exist.**

The way a knowledge-structure tool most easily dies is by filling its own blanks — because "filled"
looks more useful than "empty". Once it does that, it degrades into yet another machine that
turns what you don't know into something that looks like what you do — the exact thing it opposes.

So this rule is enforced at **two independent points**, neither of which can be bypassed:

| Enforcement point | Mechanism |
|---|---|
| **The validator** | Rule R7: if `filled_by` says a model and `evidence_status: 已验证` appears, it is a hard ERROR whose message explicitly flags "**suspected AI fill-in**" |
| **This README** | Every filler — human, model, or script — must state `filled_by` truthfully. Model-written content says `模型(vX)`; every sample here says `模型(示例数据)` |

**If you want to use AI to help populate this repo, have it write `filled_by` as itself.**
That is not a prohibition, it is a trace. Once traced, anyone can see how cautiously that
piece of knowledge should be treated.

**The value of `filled_by` is deliberately unrestricted.** The validator requires the field to be
present; it does not prescribe what may go in it. The `人` / `模型(vX)` forms used here are a
**convention, not a whitelist**. If you want to plug in your own fillers — another model, a
script, an institutional process — define your own values and enforce them in your own validation.
You do not have to change this spec.

**The one thing guaranteed here: whatever gets filled must be traced, and the trace must show who
filled it.**

There is exactly one exception: `未填充` ("not filled"). It is not a way of signing off — it is a
**factual assertion**: *nobody has filled this node yet*. That gives it a meaning the structure can
check: **a node declaring `未填充` must contain no content**, or it fails `R8`. Without that rule,
`未填充` would become a legal channel for "filled, but unsigned", and the sentence above would
shrink to a matter of good faith.

The moment **untraced model fill-in** appears, this project's first hard rule has failed —
and that is far more serious than any validator error.

## Five-minute start

```bash
git clone https://github.com/aic-123/Scaffold.git
cd Scaffold
pip install -r requirements.txt      # PyYAML, and that's the whole list
python validator/validate.py ./samples
```

**Verify the validator itself first:** `python validator/validate.py --self-test`
(reads no data files, just runs its built-in assertions).

Exit code: 1 if there are `ERROR`s, 0 if only `WARN`s.
**`samples/` passes cleanly, exit code `0`.** The blanks only produce `WARN`s — those are not
errors, they are the very thing this tool exists to show.

One command, and you get a report like this (the **complete** real output below — not a mock-up,
not an excerpt):

```
samples/roommate-noodles/claim-0101.md:6  WARN  R6  cues 是空数组——没有情境索引，认局的时候调不出这个节点。
    ↳ 修复方向：补上「当事人会怎么描述自己的处境」。确实写不出就先留着，这只是警告。
samples/roommate-noodles/cond-0101.md:6  WARN  R6  cues 是空数组——没有情境索引，认局的时候调不出这个节点。
    ↳ 修复方向：补上「当事人会怎么描述自己的处境」。确实写不出就先留着，这只是警告。
samples/router-reset/cond-0001.md:6  WARN  R6  cues 是空数组——没有情境索引，认局的时候调不出这个节点。
    ↳ 修复方向：补上「当事人会怎么描述自己的处境」。确实写不出就先留着，这只是警告。

扫描 23 个节点 —— ERROR 0，WARN 3，INFO 0
本报告只诊断结构是否自洽，不判断内容对错，不排序，不建议采信任何一条。
```

*(The validator speaks Chinese. All three findings above are the same one: `WARN R6 — cues is an
empty array: no situation index, so this node won't be called up when you're sizing up a
situation.` It is a warning, not an error, and the fix direction is "add how the person involved
would describe their own situation — and if you genuinely can't, leave it.")*

**The WARNs produced by empty values are the product demo.** Those "missing source" and
"no situation index" findings are not the samples being sloppy — they are the reason this
tool exists: surface the gap, don't fill it.

### What is still owed: the to-fill list

```bash
python validator/validate.py ./samples --todo
```

The same validator, a different lens. It does not report "what is broken" — it lists only
**"what is still owed"**: nodes whose `cues` are empty, whose `source.ref` is empty, or whose
`notes` say "to fill" — together with the relations they already hang off.

```bash
$ python validator/validate.py ./samples --todo
待填清单 —— 只列缺口，不给答案，不按任何权威度排序。

samples/roommate-noodles/arg-0102.md
    论据　「沉默的原因里，没有一条是同意」
    · 缺 source.ref —— 说不出这条知识从哪来
    · 缺 notes —— 自己标记了「待填」
    · 已挂上：stance-0102

samples/roommate-noodles/case-0101.md
    案例　「他没回我，我就吃了」
    · 缺 source.ref —— 说不出这条知识从哪来
    · 已挂上：judge-0101 → case-0001

samples/roommate-noodles/claim-0101.md
    断言　「问过之后吃，比不问就吃更接近做对」
    · 缺 cues —— 没有情境索引，认局的时候调不出这个节点
    · 缺 source.ref —— 说不出这条知识从哪来
    · 缺 notes —— 自己标记了「待填」
    · 已挂上：issue-0101
...
扫描 23 个节点，21 个存在待填项。
注意：列出缺口不等于出错。「未验证 + 无出处」是合法状态，列在这里只因为它还欠着。
这份清单只说「还欠什么」，不说「该填什么」。填什么、由谁填，由人决定。
```

*(Only the **first 3 of the 21 nodes** are shown above; the ellipsis stands for output that
genuinely exists — it is not a shorthand.)*

*(Look at the second entry: it is a **case**, and its only gap is `source.ref`. That is "empty
values are legal" doing actual work — a case that cannot name its source is still a perfectly
legal node. It simply cannot be used as evidence. That is a different thing from being broken.)*

*(In English: the header reads "to-fill list — lists gaps only, gives no answers, sorted by no
notion of authority". Each entry names the file, then the node's type and title, then the gaps —
missing `source.ref` ("can't say where this knowledge came from"), missing `notes` ("marked
itself as to-fill"), missing `cues` ("no situation index, so it won't be called up when you're
sizing up a situation") — and finally the relations it already hangs off. The footer: 23 nodes
scanned, 21 with outstanding items; listing a gap is not an error — "unverified + no source" is
a legal state, and it appears here only because it is still owed; this list says "what is owed",
never "what to write". What to write, and who writes it, is a human decision.)*

**Note that last line**: this list **gives no answers**. It tells you where the blanks are, but
*what to write there* must be decided by a human — which is exactly the usable outlet of the
"empty values are legal" hard rule. The exit code is always `0`: a gap is not an error.

What it looks like:

```
                        ┌──────────────────────────────────────┐
                        │  cues     situation index ← entry    │
                        │  title    concept index   ┐          │
                        │  aliases  aliases  ┘ ← used later    │
                        └────────────────┬─────────────────────┘
                                         │
                                ┌────────▼─────────┐
                                │  node = one .md  │
                                │                  │
                                │  id              │  prefix-NNNN
                                │  type            │  one of nine
                                │  scope           │  incl. "not applicable to"
                                │  source.ref      │  ← may be empty
                                │  evidence_status │  ← defaults to 未验证
                                │  relations ──────┼──▶ another node
                                │  filled_by       │  ← who filled it, truthfully
                                └──────────────────┘
                                         │
                          ┌──────────────┴──────────────┐
                          │  validator/validate.py      │
                          │  reports only; never writes │
                          └─────────────────────────────┘
```

## Sample skeletons

Two **entirely fictional** domains under `samples/`, 23 nodes in total (11 + 12):

| Domain | The everyday question |
|---|---|
| [`samples/router-reset/`](samples/router-reset/) | When to reboot the router vs. when to file a repair |
| [`samples/roommate-noodles/`](samples/roommate-noodles/) | Whether to eat your roommate's instant noodles |

Both cover **exactly six shapes**:

1. A complete **decision point**: ≤4 variables + thresholds + exceptions
2. An **unconverged disagreement**: an issue + two opposing stances + arguments for each
3. **At least 2 plainly visible empty values**: `source.ref: ""`, `evidence_status: 未验证`, `notes: 待填`
4. One **exception-override** relation carried in `relations`
5. **Concept nodes carry `aliases`** (the vocabulary layer's altLabel): "假死" has the aliases
   "假掉线" and "假连接" — this is where "one concept, three names" actually lives
6. A **case** (layer ⑤): situation features + options considered + reasoning + **an empty
   outcome** + a counterfactual. The two cases are joined by `relations`
   (`case-0101 → case-0001`, **one direction only**) — not because the situations are alike
   (one is a router, one is instant noodles) but because they leave **the same field empty**:
   the outcome is unknown, so the case can be retrieved and cannot be copied.
   (Neither a case's body shape nor its `relations` direction is validated — see
   "What we do NOT constrain" above.)

**Every sample's `filled_by` is `模型(示例数据)`** ("model, sample data") and every
`evidence_status` is `未验证`. A node's `notes` uses one of exactly three phrasings: a shape demo
marker ("not real knowledge"), the "to fill" marker that `--todo` looks for, or — on the two case
nodes — `"演示用，非事件记录"` ("demo, not an event record"). **They are not records of anything
that happened.** No real textbooks, papers, people, institutions, or product models. I made these
up — read them as shapes, not as knowledge.

**This directory passes cleanly** (exit code `0`). Its only findings are 3 `WARN`s from empty
`cues` — the normal state of an unfilled field, not an error.

### Want to see what the validator catches: `samples-broken/`

```bash
python validator/validate.py ./samples-broken
```

[`samples-broken/`](samples-broken/) is a **deliberate error set**: one file per rule.
Run it and all of R1–R8 fire (10 nodes, ERROR 8 / WARN 1, exit code `1`).
Each file's body spells out "what's wrong, why it's wrong, how to fix it".

The split between the two directories:

| Directory | What it is | Exit code |
|---|---|---|
| `samples/` | **Shape demo** — structurally complete, passes clean | `0` |
| `samples-broken/` | **Rule demo** — each file deliberately breaks one rule | `1` |

The reason for the split is simple: the first command should be a green light.
If you want to see the validator's teeth, go look at the second directory —
**"getting started" and "error report" don't have to be squeezed into the same command.**

## Disclaimers and coverage

- This repo contains no real domain knowledge. All samples are fictional
- This tool offers no professional advice and replaces no domain expert
- The validator checks structural consistency only. **Passing does not mean the content is correct**
- Not covered: retrieval, content library, version migration, **i18n**, permissions, concurrency,
  verifier integration
  (**aliases** are now carried by the `aliases` field; **multilingual labels** remain uncovered —
  see [`SPEC.md`](SPEC.md), "Vocabulary layer gap")
- Not yet validated: whether this structure holds up on real, large, genuinely contested knowledge

## Maintenance

Best-effort. **Issues are triaged once a month.** No SLA, no response-time guarantee, no fix deadline.
This is a demo, not a project.

Before sending anything back, read [**CONTRIBUTING.md**](CONTRIBUTING.md) (Chinese). It clears up two
things that are easy to get backwards:

- **Upstream does not take real domain content.** Deciding what to include is itself an authority
  endorsement, which collides with this project's first design principle. Keep your domain
  knowledge in your own fork
- **Adding a rule is held to a higher bar than adding a field.** Rules bind everyone, and they push
  existing users to go edit their own files. A new rule needs the designer's explicit sign-off

Version history lives in [`CHANGELOG.md`](CHANGELOG.md).

## License

**Apache-2.0**, see [`LICENSE`](LICENSE).

Two reasons for this choice:

1. **An explicit patent grant** — Section 3 gives every contributor a clear patent license.
   This is the clause corporate legal teams care about most
2. **An explicit non-grant of trademarks** — Section 6 states this license grants no trademark rights

The second is a deliberate opening: it preserves the independence of any future
"**verified**" certification mark. Anyone may use this structure, but no one may claim
certification merely by using it — that status can only be granted by an independent verifier
(see [`hooks/validator-interface.md`](hooks/validator-interface.md)).

> This repo does **not** use "for personal study only / non-commercial" style terms. That is not
> open source (it is source-available), and such restrictions kill adoption. The protection here
> comes from **disclaimers**, not from usage restrictions.

---

## For players who get it

> This is just a demo. The architecture (six layers) is a capability ceiling, not a requirement —
> merge, trim, or replace any layer you like, as long as the four hard rules hold.

The four hard rules:

1. Never fill in content on someone's behalf — unknown means empty
2. `filled_by` is stated truthfully
3. `evidence_status` never accepts self-declaration
4. Authority, source credibility, and institutional prestige never enter any sort logic

Throw away the layers if you want. These four are not optional. Drop any one of them and this
repo has no reason to exist.

---

*Design draft v0 (2026-09). Cases are added by humans later; AI must not fill in **real cases**.
The two cases in `samples/` are **demo shells, not records** — their `结果` field is empty on
purpose. Do not read them as precedent.*

<!-- Copyright 2026 AIC-123 · SPDX-License-Identifier: Apache-2.0 -->
