---
title: "Compound Velocity"
sub_title: "JIT Software, Context Engineering, and the Knowledge That Compounds"
author: Jim Myers
theme:
  name: catppuccin-mocha
---

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# About Me

- **Jim Myers** — Senior Staff Site Reliability Engineer @ Ping Identity
- Using Claude Code since day one — started by chatting in the terminal
- Ended up with multiple AI agents, a knowledge vault, and a pipeline I can't stop tinkering with

<!--
speaker_note: |
  Thanks for coming out. I've got a story to tell you, and like most stories it comes in a few acts. The arc is simple: AI made software absurdly cheap to build, that turned out to be a trap, and the way out is three practices — not three tools.

  I'm Jim, Senior Staff SRE at Ping Identity. I've been using Claude Code since the day it shipped — literally the day it came out I was in the terminal poking at it. People were losing their minds that ChatGPT could write code, but as you can probably tell, I'm not really a GUI guy. I live in my terminal, so I was unimpressed.

  For the first month I was testing the waters. "Talk to it like a junior developer," they said. So I had it build apps, argued with it about what "done" means, had it fix my homelab, rewrite my dotfiles. But I kept pushing — "what else can this do?" — and somewhere along the way I built something I didn't expect. And then I watched the ground shift under it. That's what we're here to talk about.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# The Velocity Illusion

**93% of developers use AI. Productivity gains? About 10%.**

- **92.6%** use an AI coding assistant at least monthly
- **26.9%** of production code is now AI-authored
- Productivity gains **haven't budged past 10%**
  *(Laura Tacho, DX — 121,000 developers, 450+ companies)*

And worse — developers **feel** 24% faster while actually running **19% slower.**
*(METR, randomized controlled trial, 2025)*

<!--
speaker_note: |
  Let me start with the number that started this whole rewrite for me.

  Laura Tacho — CTO at DX — studied 121,000 developers across 450 companies. 93 percent use AI coding assistants. Over a quarter of production code is now AI-authored. And productivity gains haven't moved past ten percent. Near-universal adoption, a quarter of the code, and we're barely moving the needle.

  And it gets worse. METR ran a randomized controlled trial with experienced open-source developers on their own repos. They *felt* 24 percent faster. They were actually 19 percent *slower*.

  Sit with that gap. The feeling and the reality point in opposite directions. Now, full disclosure — METR tried to re-run this in early 2026 and couldn't get a clean measurement anymore: developers refuse to work without AI now, and their current read is that AI probably does speed you up. But the perception gap — feeling 24 percent faster while measuring slower — that part is real and undisputed. And that gap is the whole talk. Everybody in this room can feel the speed. The question is whether any of it is the kind of speed that lasts. So that's what I want to give you a vocabulary for today: what actually compounds, and what just feels fast.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# What Actually Compounds?

Speed you can *feel* is not the same as speed that *lasts*.

Tools change every quarter. Rate limits, models, harnesses, vendors — all of it.

So I'm not going to sell you my tools.

**I'm going to give you three practices that survive every tool change:**
just-in-time software, context engineering, and compounding knowledge.

> And yes — the blueprint and the code are open source. You'll leave with a starting point.

<!--
speaker_note: |
  Here's my thesis, up front, so you can hold me to it.

  The speed you can feel — the autocomplete, the "wow it wrote the whole function" — that's real, but it's local and it's fragile. The speed that lasts is a different animal, and almost nobody is measuring it.

  I built an elaborate system to chase the lasting kind. I'll show it to you — it's all open source, you'll leave with a working blueprint. But I want to be honest about something from the start: half of what I built is already obsolete. Providers changed their rate limits, the tools moved, and the specific machine I built needs a rewrite. We'll come back to that at the end, because it's not a footnote — it's the point.

  So I'm not here to sell you my tools. I'm here to give you three practices that outlive any tool: build software just-in-time, engineer the context, and compound the knowledge. Everything else in this talk hangs on those three.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# The Cost of Software Fell Through the Floor

The marginal cost of a working tool: **a week → an afternoon.**

> "When the cost of building drops near zero,
> **not building** a bespoke tool becomes the real waste."

Karpathy calls it Software 3.0 — the prompt is the program;
the context window is the runtime.

<!--
speaker_note: |
  Start with what actually changed, because it's bigger than "AI writes code."

  The marginal cost of producing a working, specific tool collapsed. Something that used to be a week of engineering — enough that you'd need a real business case to justify it — is now an afternoon. Sometimes less.

  Andrej Karpathy framed this as Software 3.0. 1.0 was code you wrote. 2.0 was weights you trained. 3.0 is the prompt — you program in English, and the context window is the runtime. I don't need you to buy the whole framing. I just need you to notice that when the cost of building drops toward zero, the math on *what's worth building* completely changes. Not building the little tool becomes the waste.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# The Graveyard of Half-Finished Scripts

Every developer has one.

- `prettify.sh` — half-written, never polished
- `bd-helper` — meant to wrap it, got busy
- That alias with `# TODO: handle edge case` from 2022
- The tool you always wished existed but never had time to build

These didn't die because they weren't useful.
**They died because polishing a throwaway into something you'd trust cost too much.**

<!--
speaker_note: |
  Every one of you has a graveyard. That folder of half-finished scripts you started because you hit a real problem, and then work got in the way, and you never came back. The prettify tool that's mostly working. The wrapper you were going to polish. The alias with a TODO from three years ago.

  Here's the thing about the graveyard: none of those tools died because they were bad ideas. You already decided they were useful — that's why you started them. They died because the cost of turning a throwaway script into something you'd actually trust — tested, documented, reliable — was too high to justify on a Wednesday afternoon.

  That cost is exactly what collapsed.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# Practice 1: JIT Software

**Just-in-Time tooling:** build exactly what you need, when you need it.

> Software generated and executed at the last possible moment,
> for a specific task boundary.

The interesting tools are the ones you'd **never have justified before.**

Not one-off scripts. **Shipped, documented, reusable tools** —
on the timescale of a well-written Slack message.

<!--
speaker_note: |
  I want to name what this is, because the framing matters. This is Just-In-Time software. Build exactly what you need, when you need it.

  Pre-AI, you justified a tool if it paid back weeks of engineering time. Post-AI, with a decent process, the payback threshold drops to minutes. And the interesting consequence isn't that you can build the big tools faster — it's that you can finally justify building the *small* ones. The tool that would've saved you ten minutes a week was never worth a week to build. Now it's worth an afternoon, and you come out ahead in a month.

  And I don't mean throwaway scripts. I mean shipped, documented, versioned tools, with tests, that you'd hand a teammate — produced on the timescale of writing a long Slack message.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# So I Built a Factory

If one JIT tool takes an afternoon, what happens if you build a **pipeline that makes them?**

```
  idea ──▶ refine ──▶ plan ──▶ implement ──▶ review ──▶ ship ──▶ compound
```

**12 open-source tools in 3 months.** Most born from a Slack message or a broken script.

This is the blueprint. It's all open source. *(Repos at the end.)*

<!--
speaker_note: |
  So here's where I went. If one JIT tool takes an afternoon, what happens if I stop hand-building each one and build a pipeline that produces them? A factory for tools instead of a one-off script generator.

  Over three months I shipped twelve open-source tools through this pipeline. Twelve. Each one born from a real need — a Slack message to myself, a broken step in some other workflow. Things that would have stayed in the graveyard forever.

  This is the blueprint the abstract promised you, and it's all open source — repos at the end. But before I teach you the factory, I have to tell you what the factory taught me. Because the first version of it nearly ate me.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# The Bottleneck Was Never Typing Speed

> "Software has never been constrained by typing speed.
> It has always been constrained by **cognitive load.**"

The real friction:

- **10 hours/week** searching for information — more than a full workday *(Atlassian, 2025)*
- Finding information has **passed technical debt** as the #1 source of developer friction
- Up to **45 minutes** to recover full context after a switch

**AI writes code faster. But writing code was never the bottleneck.**

<!--
speaker_note: |
  So if everything got so cheap, why are the productivity numbers flat? Because we sped up the one part that was never the bottleneck.

  Software has never been constrained by typing speed. It's constrained by cognitive load — by understanding. Atlassian found developers spend ten hours a week, more than a full workday, just *searching for information* across docs, Slack, Jira, code. Finding information has now passed technical debt as the number-one source of developer friction. It takes up to 45 minutes to recover full context after a single interruption.

  AI made the typing part instant. But the typing part was never what was slow. So when you pour a fire hose into the cheap part and do nothing about the expensive part — understanding, discovery, validation — you don't get faster. You get a bigger pile to understand.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# 73,655 Lines I Never Read

**Rig v1 — my cautionary tale.**

- 73,655 lines of AI-generated Go, across many agent sessions
- It compiled. Tests passed. **I had never read the code.**
- ~40% was infrastructure for features that didn't exist

> "AI-generated code without understanding is **technical debt at light speed.**"

Understanding debt is distinct from technical debt:
tech debt is code that's hard to *change*;
**understanding debt is code that's hard to *reason about.***

<!--
speaker_note: |
  Let me tell you about the mistake that taught me more than anything else in this talk.

  I used AI to build a CLI called Rig. Version one. Seventy-three thousand, six hundred and fifty-five lines of Go, generated across dozens of agent sessions. It compiled. The tests passed. And I had never actually read the code. I got high on the momentum.

  Forty percent of it was infrastructure for features that didn't exist. Plugin systems for plugins nobody would write. Config frameworks for options nobody would set. I had to throw it away and start over.

  AI-generated code without understanding is technical debt at light speed. And there's a sharper way to say it: this is *understanding debt*, which is different from technical debt. Technical debt is code that's hard to change. Understanding debt is code that's hard to *reason about*. You can have a clean, well-factored codebase that no human on the team can actually reason about — and that's worse, because nobody can even see the risk. The lesson isn't "don't use AI." The lesson is: the human has to remain the architect.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# The Bottleneck Didn't Disappear — It Moved

From **writing** code to **reviewing** it.

- **98% more PRs** merged — but review time **+91%**, duplicated code **8×**,
  refactoring **−60%** *(Faros AI — 10,000+ developers, 1,255 teams)*
- CircleCI: main-branch success rate at a **five-year low (70.8%)**

> "The bottleneck didn't disappear.
> It moved from writing code to reviewing code.
> **Nobody updated the scoreboard.**"

The modern developer's job shifts: **writer of code → synthesizer of information.**

<!--
speaker_note: |
  So zoom out from my one disaster to the industry. When you make production cheap and do nothing about comprehension, the bottleneck doesn't vanish. It moves.

  It moved from writing code to reviewing it. Faros AI measured this across ten thousand developers: teams with high AI adoption merge ninety-eight percent more PRs — that's the headline leadership sees. But review time is up ninety-one percent, duplicated code is up eight-fold, refactoring is down sixty percent — and there's no significant correlation with company-level delivery improvement. CircleCI reported main-branch success rates at a five-year low — around seventy percent. The work didn't get easier. The hard part just relocated, from your keyboard to your eyes — and nobody updated the scoreboard, so we all still feel productive.

  The job itself is changing shape. The modern developer is less a writer of code and more a synthesizer of information — and an editor of machine output. Hold onto that. It's why review becomes one of the three moves later.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# Why It Decays

**Judgment forms through struggle. AI removes the struggle.**

> "Long-term memory forms through struggle.
> The brain stores what it processed with difficulty;
> it discards what came easily."

And the output hides the gap:

> "The social cue that used to say *'stop, read this carefully'* is gone."

AI-generated code is formatted to your conventions, named after your variables.
**A novice now produces work that doesn't betray the novice.**

And it's measured: AI-assisted developers scored **17% lower** on comprehension
of code they'd just shipped — the biggest gap in **debugging** *(Anthropic RCT, 2026)*

<!--
speaker_note: |
  Here's the mechanism underneath the numbers. Why does naive AI use erode you?

  Because long-term memory forms through struggle. Your brain stores what it had to fight for and discards what came easy. The ten years you spent debugging bad systems and rebuilding them — that struggle is *where the judgment came from*. When AI removes the struggle, it also removes the thing that was quietly building your expertise. A developer with ten years of AI-mediated experience can end up with the judgment of a three-year developer who actually suffered through it.

  And this is measured now. Anthropic — the model vendor — ran a randomized controlled trial and found developers using AI scored seventeen percent lower on comprehension of code they had *just shipped*. About two letter grades. The biggest gap was in debugging — exactly the skill you need most when a growing share of the codebase is AI-generated.

  And the decay is camouflaged. In the old world, a junior's code *looked* like a junior's code — odd structure, weird names — and that was a social cue that said "stop, read this carefully." AI-generated code is formatted to your conventions and named after your own variables. A novice now produces work that doesn't betray the novice. The competence in the output belongs to the model, not the person. The cue that said "slow down here" is gone.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# Naive JIT Compounds Downward

```
 understanding
   │‾·._
   │    ‾·._         more code shipped
   │        ‾·._     less understood
   │            ‾·._
   │                ‾·._
   └─────────────────────▶  cycles
```

Each cheap cycle adds code faster than you add comprehension.
**Left alone, the curve bends the wrong way.**

The whole rest of this talk is how to bend it back up.

<!--
speaker_note: |
  Put it together and you get a curve — and it's pointing the wrong way.

  Every cheap cycle adds more code than you can absorb. Understanding falls behind output, and the gap widens with each iteration. That's compounding too — it's just compounding *downward*. More volume, less comprehension, a little more dependence, every single cycle. Left completely alone, that's the default trajectory of AI-assisted work. That's where the flat productivity numbers come from.

  So this is the turn in the talk. Everything from here is about bending that curve back up — deliberately, because it will not happen on its own. Three practices. The first one is the one that gave the whole thing a name.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# The Reverse Salient

> Electric motors were ubiquitous by 1900.
> Factories didn't see productivity gains until the **1920s** —
> when they were **redesigned around the technology.**

We have the models. **We lack the harness.**

- Even at frontier labs, people delegate a *fraction* of their work to AI
- The **capability overhang** is real — models can do far more than we deploy
- The bottleneck is the **infrastructure around the model**, not the model

<!--
speaker_note: |
  There's a concept from the history of technology called the reverse salient. Electric motors were everywhere by 1900. But factories didn't see real productivity gains until the 1920s — twenty years later — when they stopped bolting motors onto steam-era layouts and *redesigned the factory* around electricity.

  That's exactly where we are with AI. The models are extraordinary, and we're using them like steam-era factories used motors — bolted onto workflows designed for a world where typing was the constraint. There's a huge capability overhang: the models can already do far more than any of us are deploying them to do.

  The bottleneck isn't intelligence anymore. It's the infrastructure around the model. The redesign. And that redesign has a name.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# Practice 2: Context Engineering

> "Most agent failures are not model failures anymore.
> **They are context failures.**" — Phil Schmid

Not a prompt. A **system** that runs before the model call:

- **Dynamic** — assembled on the fly for the task at hand
- **Precise** — the right information, not the most information
- **Format matters** — a tight summary beats a raw dump

> Karpathy: "the delicate art of filling the context window
> with just the right information for the next step."

<!--
speaker_note: |
  This is where I found the vocabulary for what my pipeline was already doing. Context engineering.

  Phil Schmid put it cleanly: most agent failures are no longer model failures. They're context failures. The model is capable. What you put in front of it decides whether it succeeds.

  And context engineering is not prompt engineering. It's not finding the magic words. It's building a *system* that assembles the right information, in the right format, at the right time, before the model ever runs. Dynamic, precise, and well-formatted — a concise summary beats a giant data dump every time. Karpathy called it the delicate art of filling the context window with just the right information for the next step. Notice the word "just" — not "all."
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# Context Rot Is Real

More context is not better context.

- Across **18 frontier models**, performance **degrades as input grows** —
  even on trivial tasks *(Chroma, 2025)*
- Distractors compound; placement matters; structure can hurt
- Claude models tend to **abstain** rather than hallucinate — but the rot is universal

**So: fresh, precise context per stage. Not one bloated session that rots.**

<!--
speaker_note: |
  And here's the empirical backing, because this is the part people resist. The instinct is "give the model everything and let it sort it out." That's wrong, and it's measurably wrong.

  Chroma ran a study across eighteen frontier models — GPT, Claude, Gemini, the rest. Performance degrades as input length grows, even on trivial retrieval tasks. Add distractors and it gets worse. Where you put the information in the window matters. Sometimes more *structure* even hurts. The one nice finding: Claude models tend to abstain rather than confidently hallucinate. But the rot itself is universal.

  So the move is the opposite of the instinct. Don't build one giant session that slowly rots. Give each stage fresh, precise context for the job in front of it. Which is exactly what a staged pipeline does.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# Context Is Infrastructure

> "The agent's quality ceiling is set by the harness.
> **The leverage point isn't the model — it's the layer beneath it.**"

Martin Fowler: **Agent = Model + Harness.** Two kinds of controls:

- **Guides** (feedforward) — `CLAUDE.md`, ADRs, the plan that gates implementation
- **Sensors** (feedback) — review pipeline, tests, CI, the compound step

> Guides without sensors is wishful thinking. Sensors without guides is firefighting.

<!--
speaker_note: |
  And here's the reframe that changes how you invest. Context isn't a per-session setup cost you pay and throw away. It's infrastructure. You build it, you maintain it, and the gap between a well-organized context layer and an ad-hoc one *compounds over time, just like code infrastructure.* The agent's quality ceiling is set by the harness. A great model on a bad harness produces plausible, wrong diffs. A modest model on a great harness produces correct ones. The leverage point isn't the model — it's the layer beneath it.

  Martin Fowler gave this a name: harness engineering. Agent equals model plus harness, and the harness has two kinds of controls. Guides steer the agent before it acts — your CLAUDE.md, your ADRs, the plan document that gates implementation. Sensors observe after it acts — the review pipeline, tests, CI, the compound step.

  You need both. Guides without sensors is wishful thinking — you wrote the rules and nobody checks. Sensors without guides is firefighting — you catch problems you never prevented. The uncomfortable part: prompt-level guides get followed maybe a third of the time. So the structure has to enforce them, not just request them.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# Routing — Send Each Stage to Its Strongest Model

Each stage gets the model that's strongest at it. Today's cast:

| Role          | Today's tool   | Why                                            |
| ------------- | -------------- | ---------------------------------------------- |
| Implement     | Gemini         | Solid mid-level engineer — follows the plan, ships |
| Challenge     | Claude         | Principal-engineer energy — asks the hard questions |
| Code review   | Codex          | Best reviewer I've found — I'd never let it implement |

> The **roles** are the design. The **names** are this quarter's casting.
> Route to strengths — and expect the cast to change.

<!--
speaker_note: |
  Okay — context engineering in practice. Three moves: routing, provenance, review. Start with routing, because it's the most context-engineering thing you can do: give each stage exactly the model that's strongest at it, and nothing else.

  Today, that's Gemini implementing — it's a solid mid-level engineer, follows the plan, ships, and the request-based limits make the volume affordable. Claude challenging — principal-engineer energy, asks the uncomfortable questions, finds the gap in the plan. That's what you want from a reviewer and exactly *not* what you want from your implementer, who'd second-guess everything and never ship. And Codex reviewing — the sharpest code reviewer I've found, and a terrible implementer. Each at its strongest, nothing else.

  But here's the durable part, and I mean this as the through-line of the talk: the *roles* are the design. The *names* are just this quarter's casting. Six months ago this table looked different. Six months from now it'll look different again — and honestly, the economics that make this exact lineup work are already shifting under me. Don't memorize my cast. Internalize the principle: route to strengths, and assume the strengths will move.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# Provenance — Capture the *Why*, Not the *What*

**Shell history:**
```
  git checkout -b feat/rate-limit
  gemini /refine "rate limit the API"
  $EDITOR internal/server/handler.go
  git commit -m "Add rate limiter"
```

**A versioned event — the compound article:**
```
  Subject: Rate limiter — token bucket over sliding window
  Why: sliding window was O(n) under our p99 burst; token bucket
       is O(1) and the fairness gap was acceptable. Plan round 2
       caught this; round 1 missed it.
  Trap: round 1 defaulted to sliding window — the "obvious" HN choice.
```

> Shell history is archaeology. The article is a memo from the engineer who did the work.

<!--
speaker_note: |
  Second move: provenance — and this is the one the abstract calls correlating shell history with versioned events. Let me make it concrete.

  My first attempt at an automated retrospective was the obvious one: parse my shell history, correlate it with the git log, feed that to the retrospective step. It worked. And it didn't. Shell history tells you *what* happened — the commands, the files, the commits. It can never tell you *why*: why the plan called for three rounds instead of two, why we switched approaches at step four. And the *why* is the only part you actually want, because the *what* is already in the diff.

  So I flipped it. Instead of reconstructing intent after the fact, each stage posts a structured record *while it's working* — what it was asked, what it decided, why, what it produced, what it handed off. Left side here is what shell history captures. Right side is a real compound article: it names the algorithm, why that one and not the obvious one, which planning round caught the gap, and a trap for next time.

  Now — full honesty, and this is the through-line again. I built the substrate for this out of NNTP, IRC, and a SQL layer, all local. That specific stack is already aging out; I'd build it differently today. But the principle is permanent: capture intent as a first-class, versioned artifact, not a transcript you mine later — so the history captures *why*, not just *what changed*.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# Review — Make It Multi-Dimensional

The bottleneck moved to review. So make review **orthogonal.**

```
  Code ─▶ Codex      (pre-PR, multiple rounds)
       ─▶ Claude     (architectural pass)
       ─▶ CodeRabbit  + Copilot   (PR-time, async)
       ─▶ Me         (final — I own this code)
```

Not redundant. **Orthogonal** — different training, different blind spots.

> If they find nothing, I lost a few minutes. If they find something, I dodged a bug.

<!--
speaker_note: |
  Third move: review. Remember — the bottleneck moved to review. So review is where you invest, and the move is to make it multi-dimensional.

  Several independent reviewers, each with different training data and different blind spots. Codex does pre-PR rounds; I take its nits back to the implementer and the code tightens each pass. Claude does an architectural read. Then CodeRabbit and Copilot run async on the PR — and yes, I run both, because they consistently catch different things. And the last reviewer is always me, because I'm the one who has to own this code, and that's the lesson from the 73,000 lines.

  The point isn't that more reviewers beat one. It's that *different* reviewers catch what any single reviewer — human or AI — would normalize to and skip right over. It's cheap insurance. If they all find nothing, I'm out a few minutes. If one finds something, I just dodged a production bug. That validates the AI-generated code before it ships — which was the promise.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# When Code Is Cheap, the Spec Is the Asset

> "When code generation is cheap,
> the bottleneck moves **upstream to specification.**"

The review cliff:

> Write **20 lines of prompt**, get **5,000 lines of code** —
> and the economics of review change completely.

The implementation is now disposable. **The spec is the durable artifact.**
Spec before code. Document intent before generating it.

<!--
speaker_note: |
  One more piece before the payoff, and it's the strategic consequence of everything so far. When code generation is cheap, the bottleneck moves upstream — to specification.

  Federico Tomassetti named the trap as a cliff: you write twenty lines of prompt and get back five thousand lines of code. The leverage is incredible, but the economics of review just exploded — you have to review five thousand lines you didn't write. The way out isn't to review harder. It's to invest upstream, where it's cheap: get the spec right before you generate, because reviewing a prompt or a plan before generation is far cheaper than reviewing generated code that mis-implemented a requirement.

  So flip your sense of what's precious. The implementation is now the disposable part — you can regenerate it. The spec, the plan, the intent — that's the durable artifact. Spec before code. Every time.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# Practice 3: The Compound Step

**The most important step in the entire pipeline.**

After every merged cycle, run `/compound`:

- A mini retrospective on the completed work
- Writes **lessons, patterns, and traps** — as real, durable files
- Writes key findings back into `CLAUDE.md` / `GEMINI.md`

Every other step ships the feature.
**This step means the next feature ships better.**

<!--
speaker_note: |
  This is the one I want you to actually remember. If you forget everything else, keep this.

  Every step up to now makes the *current* task better. This step makes every *future* task better. That's a completely different kind of return.

  After a PR merges, I run the compound step. It runs a mini retrospective on the work — the ticket, the plan, the implementation, the review. And it writes at least three things as durable files: what we learned, patterns worth repeating, and traps to avoid. Then it folds the key findings back into the CLAUDE.md and GEMINI.md, so the next session — either agent — starts with that knowledge already loaded.

  Without this step, every session's knowledge dies with the session and the next one starts at zero. That's the downward curve from earlier. This step is the thing that bends it up. Every other step ships the feature; this step means the next feature ships better.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# Contextual Equity

**Compound interest, for knowledge:**

1. **Deposit** — a task finishes; the agent logs a lesson:
   *"gRPC handshake fails on Darwin if the UDS path exceeds 104 chars."*
2. **Recovery** — six weeks later, a related task primes the agent with that note.
3. **Yield** — the agent "remembers" the limit before writing a line.

```
 throughput        ╭─────   ← compounding (knowledge persists)
   │            ╭──╯
   │        ╭──╯
   │   __╭──╯______________  ← linear (start from zero each time)
   └────────────────────────▶ cycles
```

The `darwin-uds-path-limit.md` trap is real.
**46 traps, 54 patterns** in the vault. That's the proof — not metrics, **artifacts.**

<!--
speaker_note: |
  Here's what turns a pipeline that repeats into a system that compounds. I call it Contextual Equity, and it's a compound-interest metaphor on purpose. Each completed task deposits knowledge; every future task earns interest on all the prior deposits.

  Concrete example. A task finishes and the compound step logs: "the gRPC handshake fails on Darwin if the Unix domain socket path is over 104 characters." That's a trap file; it goes in the vault. Six weeks later I start a related feature, the orchestrator primes the agent with that note, and the agent avoids the limit before writing a line. That file — darwin-uds-path-limit-dot-md — is real. It's saved me three times.

  Look at the two curves. Stateless chat is the flat line: you start from zero every session, so your throughput is linear at best. Persistent, compounding knowledge is the line that bends up: each cycle starts higher than the last. And I'm not going to wave a productivity percentage at you, because I don't have a clean RCT on myself. What I have is artifacts — forty-six trap files, fifty-four pattern files, each one written by the compound step after a real task. That's the proof. Things I learned once and never have to learn again.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# The Mechanism — and the Catch

**Four levers that compound:** Connect · Encode · **Teach** · Parallelize

> "Friction that appears once is noise.
> Friction that appears weekly across the team is a **workflow architectural defect.**"

**The catch — compounding is not automatic:**

> "Wrong claims compound — A's claim becomes B's context becomes
> C's load-bearing fact, never re-checked."

Memory rots too. **You reconcile it, or you compound the errors.**

<!--
speaker_note: |
  How do you actually drive the upward curve? Four levers. Connect — stop ferrying data between tools by hand. Encode — script the sequences you repeat. Teach — persist the context that currently only lives in your head. And Parallelize — run independent work concurrently. Teach is the one most people skip, and it's the one that compounds. The test for where to invest: friction that shows up once is noise; friction that shows up every week, across your team, is a workflow architectural defect — fix that.

  But I owe you the catch, because I promised honesty. Compounding is not automatic, and it cuts both ways. The same mechanism that compounds your good knowledge will happily compound your wrong knowledge. A stale claim becomes the next session's context, becomes a load-bearing fact three sessions later that nobody re-checks. Memory rots. So part of the practice is reconciliation — pruning, correcting, retiring stale traps. If you don't tend it, you're just compounding errors faster. The curve only bends up if you maintain it.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# The Factory Is Already Half-Obsolete

The pipeline I just showed you?

- Provider **rate limits changed** — the cheap implementer isn't as cheap
- The **tools moved** — better models, new harnesses, shifting roles
- My kernel **needs a significant rewrite** just to keep up

**This is not the sad part of the talk. This is the proof.**

If the system were the point, it'd already be failing.
The **practices** haven't moved an inch.

<!--
speaker_note: |
  And now the honest ending I promised you at the very beginning.

  This factory — the one I just spent half an hour teaching you — is already half-obsolete. The providers changed their rate limits, so the economics that made my implementer cheap don't hold the way they did. The tools moved — better models, new harnesses, the roles shifting between vendors. My kernel needs a real rewrite just to keep up with the ecosystem. And the talk's original subtitle was literally about building this specific machine.

  I want to be clear that this is not the embarrassing part of the talk. This is the *whole point* of the talk. If the *system* were what mattered, I'd be up here failing in real time. But watch what actually broke: the tools. The names in that routing table. The specific substrate. The things I told you not to memorize.

  What didn't move an inch? Build just-in-time. Engineer the context. Compound the knowledge. Every tool I swap, I carry those three across unchanged. That's the difference between speed you can feel and speed that lasts.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# What Survives the Churn

```
   TOOLS            (disposable)     PRACTICES        (durable)
   ─────                              ─────────
   Gemini / Claude / Codex      →     route to strengths
   NNTP / IRC / Dolt substrate  →     capture the why
   Honeycomb kernel             →     compound every cycle
   this quarter's rate limits   →     spec before code
```

**Just-in-time software. Context engineering. Compounding knowledge.**

Bet on the right column.

<!--
speaker_note: |
  Here's the whole talk on one slide. Left column: everything that's going to change out from under you — my models, my substrate, my kernel, this quarter's pricing. Right column: what each one is really an instance of — routing to strengths, capturing the why, compounding every cycle, spec before code.

  The mistake — the one I made, the one the flat productivity numbers are made of — is betting on the left column. Falling in love with the tool. The tools are genuinely amazing and they are also completely disposable. Bet on the right column. Just-in-time software, context engineering, compounding knowledge. Those three will still be true when every tool on this slide has been replaced twice.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# Empty Your Graveyard

It's all open source — take the blueprint and the working code as a **starting point.**

**This week:**

1. **Codify one thing** you re-explain to your agent every session (a `CLAUDE.md`, an ADR)
2. **Ship one JIT tool** — something from your graveyard
3. **Run the compound step** — write down what you learned

That first deposit is the hardest. Everything after earns interest on everything before.

```
  github.com/thoreinstein/rig              github.com/thoreinstein/honeycomb
  github.com/thoreinstein/beads-workflow   github.com/thoreinstein/beads-workflow-claude
```

<!--
speaker_note: |
  So what do you do Monday? I promised you a blueprint and working code, and it's all up there — Rig, Honeycomb, the workflow extensions, all open source. Fork it, read it, use it as a starting point. But fork it knowing it'll change, because now you know that's the deal with all of it.

  Here's what I actually want you to do this week, and none of it requires my code. One: pick the single piece of context you re-explain to your assistant every session — your architecture, your bounded contexts, your conventions — and codify it into a persistent artifact. A CLAUDE.md, an ADR, a README section. That's your first deposit of contextual equity, and it's the hardest one because you have to decide what matters.

  Two: ship one JIT tool. Something out of your graveyard — the small thing you always wished existed. Build it just-in-time, this week. Three: when you finish it, run the compound step on yourself. What did I learn that I didn't know before? Write it down where the next session will find it.

  Do that and the deposits start making themselves. Don't build my factory — it's already obsolete. Empty your graveyard, and keep the three practices when you swap the tools. Thank you.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- jump_to_middle -->

# Questions?

```
  Just-in-time software ─┐
  Context engineering    ├─▶  the tools will change.
  Compounding knowledge ─┘    these won't.
```

**The takeaway:** route to strengths · capture the *why* · compound every cycle.
Codify one thing today. Build it just-in-time. Compound from there.

Repos: `rig` · `honeycomb` · `beads-workflow` (+ `-claude`) — all open source.

<!--
speaker_note: |
  Happy to take questions, and the repos are all up there.

  The one thing to take home: the tools will change — mine already have. Just-in-time software, context engineering, and compounding knowledge won't. Codify one thing today, build it just-in-time, and compound from there.

  (If nobody jumps in first: "The question I get most is — isn't this over-engineering? And my answer is 73,655 lines of Go I never read. *That* was over-engineering. This — three practices you carry across every tool — is the opposite.")
-->

<!-- end_slide -->
