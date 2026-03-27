<p align="center">
  <h1 align="center">/next-big-thing</h1>
  <p align="center">A Claude Code skill that reads your entire codebase and tells you the single smartest thing to build next.</p>
</p>

<p align="center">
  <a href="#install">Install</a> &bull;
  <a href="#usage">Usage</a> &bull;
  <a href="#portfolio-mode">Portfolio Mode</a> &bull;
  <a href="#how-it-works">How It Works</a> &bull;
  <a href="#example">Example</a>
</p>

---

## The Problem

You have a codebase. You know it needs *something* — but what? You could brainstorm a list of 20 ideas, but lists don't ship. What you actually need is one opinionated recommendation backed by evidence from your own code, with a concrete plan to build it.

## The Solution

`/next-big-thing` silently reads your codebase — source files, git history, roadmaps, TODOs, architecture — deliberates on what would be most transformative, and delivers a single recommendation argued with conviction.

Not a list. Not options. One call, backed by your code.

## Why This Skill?

| Feature | /next-big-thing | Brainstorming | Asking ChatGPT |
|---------|:-:|:-:|:-:|
| Reads your actual source code | Yes | No | No |
| Checks git history for momentum | Yes | No | No |
| Avoids duplicating your roadmap | Yes | No | No |
| Provides implementation sketch | Yes | No | Sometimes |
| References specific files + line numbers | Yes | No | No |
| Opinionated (one answer, not a list) | Yes | No | No |

---

## Install

```bash
mkdir -p ~/.claude/skills/next-big-thing
curl -o ~/.claude/skills/next-big-thing/SKILL.md \
  https://raw.githubusercontent.com/vnnkl/next-big-thing/main/skill/SKILL.md
```

Or clone and symlink:

```bash
git clone https://github.com/vnnkl/next-big-thing.git
mkdir -p ~/.claude/skills/next-big-thing
ln -s "$(pwd)/next-big-thing/skill/SKILL.md" ~/.claude/skills/next-big-thing/SKILL.md
```

Verify it's available:

```bash
ls ~/.claude/skills/next-big-thing/SKILL.md
```

---

## Usage

In any Claude Code session, inside any project:

```
/next-big-thing
```

That's it. Claude does the rest:

1. **Deep Dive** — reads README, source files, `git log`, roadmaps, TODOs, architecture
2. **Deliberation** — evaluates candidates across leverage, timing, moat, delight, compounding
3. **The Pitch** — delivers one structured recommendation

### Output Structure

Every recommendation follows this format:

```
## The Next Big Thing: [Name]

### The Insight        — non-obvious observation from your code
### What It Is         — user-perspective description
### Why This, Why Now  — references specific commits, patterns, data structures
### Why Not Something Else — strongest alternative and why this wins

### Implementation Sketch
  Scope          — Small (hours) / Medium (days) / Large (weeks)
  Files affected — exact paths with descriptions
  Key decisions  — architectural choices with reasoning
  Rough LOC      — estimate across N files
  Dependencies   — what existing code this builds on
  Risk           — single biggest unknown
```

---

## Portfolio Mode

The real power move: run it on **all your projects at once** using parallel subagents.

```
Run /next-big-thing on each of my repos in ~/Code using parallel subagents.
Save each result as a markdown file in ./ideas/
```

This fans out 10-20 agents simultaneously. Each one independently:

- Reads the entire codebase
- Checks git history and roadmaps
- Identifies the highest-leverage addition
- Delivers a full implementation sketch

**Result:** a portfolio-wide idea backlog in ~2 minutes.

### Real Results from One Session

14 projects analyzed in parallel. Highlights:

| Project | Recommendation | Scope | Key Insight |
|---------|---------------|-------|-------------|
| Rust Bitcoin library | Light client scanning protocol | Medium | Scanning bottleneck: 200k HTTP requests for 1000 blocks. Tweak index reduces to ~33 bytes/tx |
| ML training repo | `mx.compile` for Metal kernel fusion | Small | 128GB Mac using 18GB. 25 lines to unlock 20-30% throughput |
| Agent orchestrator | Verification gates on task completion | Small | Engine marks tasks "done" on regex match. Never checks if build passes |
| 3D block explorer | Transaction flow visualization | Medium | UTXO connections as animated 3D arcs. Rendering primitives already exist |
| macOS menu bar app | Active project context detection | Medium | 126 skills shown equally. Terminal CWD detection is 80% built |

Ideal for **end-of-week token surplus** on Max plans. Turn leftover tokens into reconnaissance.

---

## How It Works

### What It Reads

| Source | What It Extracts |
|--------|-----------------|
| README, package.json, Cargo.toml | Project identity and purpose |
| Directory structure | Architecture and abstractions |
| `git log --oneline -20` | Recent momentum and direction |
| TODO, .planning/, ROADMAP.md | Existing plans (to avoid duplicating) |
| Source files | Gaps, friction, half-built features |

### Evaluation Criteria

Ideas are evaluated across five dimensions:

| Dimension | Question |
|-----------|----------|
| **Leverage** | Would this make existing features more valuable? |
| **Timing** | Is this uniquely possible now given recent commits? |
| **Moat** | Would this differentiate meaningfully from alternatives? |
| **Delight** | Would users say "I didn't know I needed this"? |
| **Compounding** | Does this get more valuable over time? |

### What It Discards

- **Table stakes** — every project has this, it's not innovative
- **Incremental improvements** — useful but not transformative
- **Architecture astronautics** — technically cool, doesn't serve users
- **Already planned** — just restating the roadmap

---

## Example

From a real run on a tvOS Bitcoin charting app:

> ### The Next Big Thing: Technical Indicators Engine
>
> **The Insight:** The app has pixel-perfect Canvas rendering and a clean Kline model with full OHLCV data — yet it renders naked candles. The entire data pipeline is built; the app just isn't using the data it already has. Adding indicators turns this from "a really nice ticker" into "the app I actually leave running."
>
> **Scope:** Medium (~600 LOC) | Zero new dependencies
>
> **Files affected:** 9 files — 4 new pure-function indicator modules (EMA, SMA, RSI, MACD), 2 new overlay views, 3 modified (ChartContainer, Settings, Theme)
>
> **Why not multi-symbol support?** It's a horizontal expansion touching 15+ files. Indicators are a vertical deepening — making the one thing the app does dramatically more useful. The ROI per line of code isn't close.
>
> **Risk:** Visual clutter on 10-foot UI. Mitigation: default to EMA-21 only, thin semi-transparent lines, test on a real TV at 10 feet.

---

## Design Philosophy

**One answer, not a list.** Proposing five ideas is easy and useless. The hard part — the part that creates value — is the judgment call. This skill makes that call.

**Evidence over intuition.** Every recommendation references specific things found in the codebase: recent commits, architectural patterns, existing data structures. No hand-waving.

**Surprise the user.** If you could have thought of it yourself, the skill hasn't looked deeply enough. The best recommendations come from connecting dots the developer is too close to see.

---

## Limitations

- Requires Claude Code (uses Explore agents, git commands, file reading tools)
- Quality scales with codebase size — very small projects may get generic suggestions
- Cannot access external issue trackers, Slack, or design docs unless they're in the repo
- One recommendation per run — use [`/idea-wizard`](https://github.com/vnnkl/agentflywheel/tree/main/skills/idea-wizard) if you want a brainstorming list
- Portfolio mode (parallel subagents) works best with Claude Code Max plans due to token usage

---

## FAQ

**Q: How is this different from [`/idea-wizard`](https://github.com/vnnkl/agentflywheel/tree/main/skills/idea-wizard)?**
[`/idea-wizard`](https://github.com/vnnkl/agentflywheel/tree/main/skills/idea-wizard) is divergent brainstorming (30 ideas narrowed to 5). `/next-big-thing` is convergent analysis — one recommendation, argued with conviction.

**Q: Does it modify any files?**
No. It's read-only. It explores and reports. You decide whether to build it.

**Q: How long does a single run take?**
Typically 60-120 seconds. It reads a lot of files and thinks hard before answering.

**Q: How long does portfolio mode take?**
About 2-3 minutes for 10-15 projects running in parallel.

**Q: What if my project is genuinely in great shape?**
It will say so. "The best thing you can do right now is ship what you have" is a valid output.

**Q: Can I run this on someone else's repo?**
Yes. Clone it locally and run `/next-big-thing` inside the directory.

## License

MIT
