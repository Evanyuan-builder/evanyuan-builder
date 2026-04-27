<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=6,11,20&height=180&section=header&text=Evan%20Yuan&fontSize=55&fontColor=ffffff&animation=fadeIn&fontAlignY=38&desc=AI%20Systems%20·%20Agent%20Infrastructure%20·%20Developer%20Tools&descAlignY=60&descSize=14" width="100%"/>

<a href="https://github.com/Evanyuan-builder">
  <img src="https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=500&size=18&duration=3500&pause=800&color=6C7B95&center=true&vCenter=true&width=620&lines=open+verification;closed+core;numbers+over+marketing" />
</a>

<p>
  <img src="https://img.shields.io/badge/focus-AI%20Systems-6C7B95?style=flat-square" />
  <img src="https://img.shields.io/badge/focus-Agent%20Infra-6C7B95?style=flat-square" />
  <img src="https://img.shields.io/badge/focus-Dev%20Tools-6C7B95?style=flat-square" />
  <img src="https://img.shields.io/badge/license-Apache--2.0-6C7B95?style=flat-square" />
</p>

</div>

---

### 🧭 How I Build

I ship verification before I ship product. I wrote the eval harness **before** trusting my own system's numbers — and the first honest run showed a 20-point gap vs. state-of-the-art open baselines. The gap was the roadmap, not the marketing. n=500 sweeps closed it; the harness still ships first.

**Three-layer strategy:** open verification · semi-open interface · closed core.
The engine stays private. The scorecard is public. Anyone can reproduce the numbers in one command.

---

### 🧩 Featured Work

#### 🔬 [`memory-core-eval`](https://github.com/Evanyuan-builder/memory-core-eval) &nbsp;·&nbsp; <sub>Apache-2.0 · public</sub>

Reproducible evaluation harness for agent memory systems. Pluggable `MemoryAdapter` protocol (three-method contract), LongMemEval runner with oracle / S splits, and classical baselines shipped in-box (BM25, Dense MiniLM, BM25+Dense RRF).

**Full n=500 head-to-head on LongMemEval-S** (session haystack, ~50 sessions/q, turn-level indexing):

| Adapter | R@1 | R@5 | R@10 |
|---|---:|---:|---:|
| BM25 | 84.7% | 93.2% | 96.2% |
| Dense MiniLM | 86.8% | 94.7% | 97.2% |
| BM25 + Dense RRF | 89.8% | 96.0% | 97.9% |
| **Memory Core** | **94.5%** | **98.3%** | **99.4%** |

Memory Core leads `hybrid-rrf` on every column at n=500. [Baselines →](https://github.com/Evanyuan-builder/memory-core-eval/tree/main/baselines)  ·  [Full report →](https://github.com/Evanyuan-builder/memory-core-eval/blob/main/reports/baselines_longmemeval_s.md)

```bash
pip install 'memory-core-eval[dense]'
mceval run --adapter hybrid-rrf --split s
```

---

#### 🧠 Memory Core &nbsp;·&nbsp; <sub>closed core · private</sub>

**Deterministic temporal memory for agent teams.** LanceDB-primary retrieval (split-leg BM25 + dense, external RRF), bi-temporal supersession, autoDream consolidation, role-scoped queries (`agent` / `cross-role` / `organizational`), MCP server integration.

n=500 head-to-head, same harness, same seeds:

- **LongMemEval-S** &nbsp;·&nbsp; R@1 **94.5%** / R@5 **98.3%** / R@10 **99.4%** &nbsp;·&nbsp; +1.5pt R@10 vs `hybrid-rrf` paper
- **LoCoMo** &nbsp;·&nbsp; R@1 **58.4%** / R@5 **81.8%** / R@10 **88.8%** &nbsp;·&nbsp; +3.8pt R@10 vs `hybrid-rrf` n=100 anchor
- **348 unit tests** · **CI 7/7 green** including a determinism job (zero question flips across server restarts at the same code revision)
- REST (FastAPI) · Python/TS SDK · MCP server · v0.3.0 released

**Three orthogonal axes** — deterministic retrieval (SQL push-down + explicit tie-breakers) · bi-temporal memory (supersession + windowed RRF on event-time) · role-scoped queries (first-class `agent` / `cross-role` / `organizational`).

The verification is public; the engine isn't. Request access if you want to integrate.

---

#### 📦 [`evancore`](https://github.com/Evanyuan-builder/evancore) &nbsp;·&nbsp; <sub>public design · closed core · Apache-2.0</sub>

**Terraform for AI agents.** CLI-first Harness Engineering — the delivery layer *between* agent frameworks (runtime) and hosting (execution). Content-addressed artifacts (blake2b-64), Ed25519 signing with per-host `trust.yaml` + revocation v2, reversible lifecycle (`assemble / release / rollback / upgrade / promote / publish / pull / verify`). MCP is one Tool backend, not the product.

- **607 unit tests** · v1 A–G + v2 open-foundation four-line runtime + B3 signing A/A.1/B/B.1/B.2 + MCP Phase 2 (persistent session, reconnect-once, per-call escape hatch)
- **Evaluation log published** — 11 ecosystem candidates evaluated, 2 accepted as references, 9 declined on fit. The defensibility of scope is itself public evidence.
- Spec schema · philosophy · examples · interactive lifecycle demo in repo

```bash
# after private-beta access:
evan release app.yaml --sign alice@acme
#  📦 SPEC_RELEASED  h-abc123f7d9e2
#  ✍  SPEC_SIGNED    alice@acme
#  🚀 HARNESS_DEPLOYED app (av)
```

The design is public, the engine isn't. [Live demo →](https://evanyuan-builder.github.io/evancore/)  ·  [Philosophy →](https://github.com/Evanyuan-builder/evancore/blob/main/docs/philosophy.md)  ·  [Evaluation log →](https://github.com/Evanyuan-builder/evancore/blob/main/docs/evaluations.md)

---

#### 📇 [`rolecore`](https://github.com/Evanyuan-builder/rolecore) &nbsp;·&nbsp; <sub>public showcase · closed core · Apache-2.0</sub>

**Role assets for LLM agents.** Treat an "agent role" the way you'd treat a production asset — a version, a lifecycle, a signed checksum, a gate before it goes live. Not a prompt string; an asset.

- **Five-level lifecycle** — `raw_imported → normalized → curated → official → archived`, forward-only state machine
- **Two gates** — 10-item structural (`official`) + 5-dimension LLM review (`curated`, Anthropic or local Ollama)
- **Three-axis fusion search** — job × tech × specialty, ranked per-axis with a triad recommendation
- **105 reference roles** shipped across 26 groups, all double-gated · **39 pytest**, all green

```bash
rolecore find "python backend testing"
rolecore assemble --id engineering.backend_architect \
    --task "design an order service" --goal "support 10k QPS"
```

The core is public; the upstream integration (four third-party agent repos, 493-role full dataset, drift monitor, batch reviewer, operations playbook) stays private. [Release v0.1.0 →](https://github.com/Evanyuan-builder/rolecore/releases/tag/v0.1.0)  ·  [Concepts →](https://github.com/Evanyuan-builder/rolecore/blob/main/docs/concepts.md)  ·  [CLI reference →](https://github.com/Evanyuan-builder/rolecore/blob/main/docs/cli.md)

---

#### 🧭 Yunzhou (运筹唯握) &nbsp;·&nbsp; <sub>cognitive workbench · private beta</sub>

A thinking environment that offers a different cognitive interface at each stage of a complex problem — not a canvas, not a whiteboard, not a note app. Five stages, one object model:

| stage | interface | purpose |
|---|---|---|
| frame | ProblemFramer | set the coordinate system for the thought |
| expand | IdeaEntryBoard | find handles, not content |
| argue | Canvas | review a case, not browse a knowledge graph |
| close | ClosureSheet | ritual closure — human confirms, AI challenges |
| evolve | JudgmentTimeline | judgment history, not operation log |

React 19 · Vite · Express · AI co-pilot "Vantage / 衡镜". Private beta — demo on request.

---

### 🛠 Also Building

**AgentForge** &nbsp;·&nbsp; Role-as-code agent framework for operator-grade AI staffing. MVP: four-layer architecture (intent / llm / boundary / handoff), 40/40 integration tests, three-system integration. Private.

---

### 📜 Principles

> **Open verification. Closed core. Numbers over marketing.**

- Build the scorecard first. If you can't score yourself honestly, your product is a demo.
- The engine is the moat. The evidence is the pitch.
- `pip install` a harness beats any claim in a deck.

---

<div align="center">

### 📊 Workshop Signals

<img src="https://github-readme-stats.vercel.app/api?username=Evanyuan-builder&show_icons=true&theme=transparent&hide_border=true&title_color=6C7B95&icon_color=6C7B95&text_color=8B95A8" height="160"/>
<img src="https://github-readme-stats.vercel.app/api/top-langs/?username=Evanyuan-builder&layout=compact&theme=transparent&hide_border=true&title_color=6C7B95&text_color=8B95A8" height="160"/>

<br/>

<img src="https://github-readme-streak-stats.herokuapp.com/?user=Evanyuan-builder&theme=transparent&hide_border=true&ring=6C7B95&fire=6C7B95&currStreakLabel=8B95A8" height="160"/>

<br/><br/>

<sub>This GitHub is my public workshop — the place I publish verification, not marketing.</sub>

</div>
