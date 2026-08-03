# STACK + AI: Talk Summary

> Reconstructed from `assets/` — the on-screen `text.md`/`word-cloud.md` content plus
> the presenter's `notes.md` files, which `stack-and-ai.txt` (a plain `present --export`
> dump) drops entirely. Notes carry most of the actual explanation; the on-screen text is
> just the cue. Where a panel's own image (`image.png`) likely carries data the text
> can't (a chart, a screenshot), it's called out but not transcribed — image text wasn't
> extractable.

## Overview

The talk traces how STACK Construction Technologies' engineering org went from an
already-strong Agile/CI/TDD baseline to restructuring around small "AI Pods" and Claude
Code over the second half of 2025 into early 2026 — what changed organizationally, what
it cost, how the team's morale shifted, and the open questions (TDD, SOLID, QA, code
ownership) the shift left unresolved.

---

## 05 — Intro

Title panel: **"STACK Construction Technologies."** The one speaker note on this panel —
"Estimate software" — reads as the presenter's own reminder of the theme they're about
to complicate: software estimation, and how AI unsettles it. Accompanied by a large
image (likely a title graphic/logo).

---

## 10 — Story Timeline

The spine of the talk — a chronological walk from "already good at this" to "restructured
around AI."

**June 2025 — "Ahead of the game."** The presenter frames this from 8 years at STACK:
the team already ran Agile, daily deployments, full CI, and TDD (the panel's word cloud)
before AI entered the picture. Notes add a pointed aside: *"Do Agile first. Also, don't do
software development without Agile"* — i.e., AI adoption built on top of process
discipline that was already in place, not a substitute for it.

**June–November 2025 — "Let's play with AI" → new VP drives focus.** Early AI use was,
in the presenter's own words, *"wrong: AI as a typing assistant"* — they tried every
vendor/model but treated AI as autocomplete. In November 2025, new investors and a new
VP of Engineering pushed a real strategic focus, which the notes summarize as three
concrete outputs: **HIP** (High Impact Pods, next panel), **Monitoring**, and
**Training**.

**High Impact Pods ("HIP" / "AI Pods" / "Mission Pods").** Agile teams taken to an
extreme: nominally 2 developers + 1 designer + 1 product owner. Notes reveal the team's
actual finding diverged from the plan — they imagined 2 developers each paired with a
Claude session, but *"actually: 1 developer for a POD works well."* Pods run with no
standing meetings: the PO drives story communication when needed, devs drive tech
communication when needed, and designers prep a UI spec then iterate directly with dev
(the notes leave "Figma?" as an open question about tooling fit).

**Jan 2026 — Marketplaces.** A claude marketplace storing all the skills/agents and mcp servers,
literally *"just a git repo"* — built to accelerate everyone's ability to adopt new
Claude-driven approaches and standardize processes/procedures across pods. Word cloud:
*Share Skills, Standardize, Accelerate, Claude.*

**Jan 2026 — MCP Servers.** Adopted MCP servers to integrate Claude with the existing
tool ecosystem — word cloud lists **JIRA, DataDog, Slack, Chrome DevTools** — favoring
what the notes call "concept integration" over strict/rigid APIs.

**Jan 2026 — "The Naive Idea."** Scaling to 25 developers (≈12 pods per the next panel's
notes) all leaning on Claude Code to write "all the code." Framed as naive in hindsight:
Product Owners had to ramp up fast, and the team learned Claude needs much more detailed
input/context than expected to be useful at that scale.

**Workflows.** A formalized flow — **research → challenge → plan → design → implement**
— backed by home-grown tooling named in the word cloud: `spec-kit`, `git-ship-done`,
`STACK sdlc`, `STACK ralph` (the last likely their name for an autonomous-agent-loop
tool, per the "Ralph Wiggum"-style looping pattern used elsewhere in this space).

**Adapting.** The closing timeline panel is explicitly unresolved — *"everything is
still changing"* — with four live work areas: **guardrails, code review, memory, token
optimization.** Notes sharpen this: moving code review to happen *at code-generation
time*, unifying scattered documentation (Confluence + repos) into one place Claude can
draw on, and actively trying to bring token costs down.

---

## 20 — Onboarding

A retrospective on how the org and its 25 developers actually absorbed the change,
including cost and sentiment data (the panels here lean heavily on charts/screenshots
that aren't transcribable — see note below each).

- **25 Developers.** Framing question for the section: *"How did they take to the new
  paradigm?"* Notes flag training and measurement as the open threads that follow.
- **Active Users Daily** *(image: DAU-style chart, not transcribed)*. Notes show the
  team wrestling with definitions as much as numbers: tracked via OpenTelemetry, with
  "what counts as a session?" still an open question as of May.
- **Usage Cost** *(image: cost chart, not transcribed)*.
- **Marketplace Skills in use** *(image: usage chart, not transcribed)* — ties back to
  the Jan 2026 Marketplaces panel; this is presumably the adoption data for it.
- **Anthropic Tokens in use** *(image: token-usage chart, not transcribed)*.
- **Budget.** Concrete numbers on-screen: **Monthly Max: Team subscrioption a mix of 
  standard and premium user costs. All rate limit exceess chaged against a max spend
  current set to $4,000**, at **72% usage by the
  18th** of the month. Notes: this cap is on top of the base subscription price, the team
  can adjust the limit themselves, and the user of guilds collaborating through Slack channels
  allows people to surface rate limit the team can actually go fix.
- **Morale.** Word cloud captures the emotional spread directly, unfiltered: *Concern,
  Excitement, "Don't like this," "I can build anything," Build models, Too expensive,
  Too dangerous.* Notes: "change is hard" — a genuinely mixed reaction, not a success
  story being oversold. It's evolved, and most appear comfortably with the change.
- **Bug Rates.** On-screen claim: bug rates *have changed* — Claude is good at
  preventing some kinds of bugs, bad at preventing *functional* bugs. Notes add a live
  design question: whether to separate "test Claude" from "code Claude" so the same
  session isn't grading its own work.
- **Today.** Closing note for the section — *"every day is a new adventure," "do the
  impossible," "teach me."* The word cloud lists the actual grab-bag of requests Claude
  now fields: writing a Swift app, debugging a failing Helm deploy, an NFS driver
  question, bugs in other people's code — illustrating the breadth, not depth, of what's
  now thrown at it.

---

## 25 — Security

- **Security.** On-screen content is literally three CLI flags, presented with a wry
  edge per the notes (*"security theater"*): `claude --dangerously-skip-permissions`,
  `claude --channels server:claude-peers`, and
  `claude --dangerously-load-development-channels server:claude-peers`. The notes'
  framing — *"protect information from identities"* vs. "security theater" — reads as
  the presenter contrasting real access control against flags that just look
  security-conscious.
- **Intentional Access.** The actual posture, per notes: no token/secret sharing with
  Claude directly, use official MCP servers only, and think about security in layers.
  Notes also raise a self-check: *"already secure? Use AI to test yourself"* — i.e.,
  turning Claude into part of the audit process, not just the risk. Word cloud names the
  categories at stake: **tokens, passwords, email?**

---

## 30 — Questions / Ideas

An open-questions section — practices the team is actively rethinking rather than ones
they've settled:

- **TDD.** *"What does this mean in the world of AI?"* Notes: TDD historically helped
  humans cover code branches and prove changes didn't break things; today AI can find those
  problems itself, so the value proposition shifts. Instead, we found value in
  proving code can runs in an environment, weighting integration tests over unit
  tests, black-box testing. Underlying problem named directly: Claude writes inaccurate
  code sometimes and needs to be "exercised" — run and checked — as feedback to itself.
- **SOLID.** *"Does it matter?"* Notes: LLMs are driven by language the same way humans
  are, so patterns and naming are still critical to how they reason about code — with
  "magic numbers?" left as an open aside. Emphasis DDD as a primary technique that carries
  more value.
- **Skill-driven development.** Start a task by first creating a project-level skill
  that iterates you through the process. Notes connect this back to the Marketplaces
  panel: workflows are good, but only if they're made into genuinely repeatable
  processes and promoted to the shared marketplace. Often a simpler skill gets things done.
- **Manual QA is back.** Terse on-screen title, notes carry the argument: *"don't trust
  Claude — you didn't write the test, how do you know?"* — a return to hands-on manual
  verification as a trust check on AI-written tests.
- **Story Review.** Stories get failed/bounced if they aren't specific enough. A story review
  skill interprets a product owners story and assigns a grade. Notes
  frame this as forcing product to define what the product should actually do, and pose
  the open question of whether product and development should define stories together.
- **Developers' Knowledge.** *"Claude writes the code. Developers know what?"* Notes
  push this into an accountability practice: a workflow step forces an interview of
  developers about code they submit, don't merge a PR if they can't pass an "open book test" 
  on it — a direct response to the risk of shipping code nobody on the team actually understands.
- **Code Review.** Reviewing code after the fact is less valuable if it can be reviewed while
  it is being written. Using file write hooks that drive appropriate standards for claude to work on
  mean we get an "correct" code when its written. We leave architecturage, cross product code
  view for CI.

