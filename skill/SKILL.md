---
name: next-big-thing
description: Analyze the current project deeply and propose the single highest-leverage, most innovative addition. Use when the user asks "what should I build next", "what's missing", "next big thing", "genius move", "what would you add", "most impactful feature", "what's the smartest thing to build", or any variation of asking for the single best idea for their project. Also use when the user seems stuck on what to work on next, or asks for strategic direction on a codebase. This skill goes deep — it reads the codebase, checks git history, and thinks hard before answering. It is NOT a brainstorming skill (use idea-wizard for that). This produces exactly one recommendation, argued with conviction.
---

# Next Big Thing

You have one job: find the single smartest addition to this project and argue for it so compellingly that the user can't wait to build it.

This is not brainstorming. You are not generating options. You are making a call — one recommendation, backed by evidence from the codebase, with a clear argument for why this specific thing matters more than anything else right now.

## Why One Thing?

Proposing five ideas is easy and useless. The hard part — the part that creates value — is the judgment call: given everything you know about this project's architecture, its gaps, its trajectory, and its users, what is the single move that would create the most compounding value?

The answer should surprise the user. If they could have thought of it themselves, you haven't looked deeply enough.

## The Process

### Phase 1: Deep Dive (silent — no output yet)

Before proposing anything, understand the project thoroughly. Use the Explore agent or read files directly. Cover all of these:

1. **Project identity**: README, package.json/Cargo.toml/go.mod, CLAUDE.md. What does this project do? Who is it for?

2. **Architecture**: Directory structure, key abstractions, data flow. Where are the seams? What patterns does the codebase use?

3. **Current state**: git log --oneline -20 for recent momentum. What has the team been building? What direction is the project heading?

4. **Existing roadmap**: Check for TODO files, .planning/ directories, issue trackers, ROADMAP.md. What is already planned? Your recommendation must not duplicate planned work.

5. **Gaps and friction**: What is missing? Where does the architecture strain? What would a user complain about? What would a senior engineer flag in a review?

6. **Ecosystem context**: What do similar projects offer that this one does not? What adjacent capabilities would create outsized value?

Spend real effort here. The quality of your recommendation is directly proportional to the depth of your understanding. If you skim, you will propose something obvious.

### Phase 2: Deliberation (silent — think hard)

With the project fully loaded in context, reason through candidates. Consider multiple angles:

- **Leverage**: What addition would make existing features more valuable? What creates a multiplier effect?
- **Timing**: What is uniquely possible now given the current architecture that was not possible before the last few commits?
- **Moat**: What would make this project meaningfully different from alternatives?
- **Delight**: What would make users say "I did not know I needed this, but now I cannot live without it"?
- **Compounding**: What gets more valuable over time as data accumulates or usage grows?

Discard ideas that are:
- Table stakes (every project has this — it is not innovative)
- Incremental improvements (useful but not transformative)
- Architecture astronautics (technically cool but does not serve users)
- Already on the roadmap (you would just be restating their plans)

### Phase 3: The Pitch

Present your recommendation with conviction. Use this structure:

## The Next Big Thing: [Name]

### The Insight
[2-3 sentences: the non-obvious observation about the project that makes
this idea compelling. This is the "aha" — the thing the user has not seen.]

### What It Is
[3-5 sentences: what the feature does, described from the user's perspective.
No implementation details yet — paint the picture of the experience.]

### Why This, Why Now
[3-5 sentences: why this specific addition matters more than anything else
at this point in the project's life. Reference specific things you found
in the codebase — recent commits, architectural patterns, existing data
structures that make this feasible.]

### Why Not Something Else
[2-3 sentences: briefly acknowledge the strongest alternative you
considered and explain why your recommendation wins.]

### Implementation Sketch

**Scope**: [Small (hours) / Medium (days) / Large (weeks)]

**Files affected**:
- path/to/file.ts — [what changes]
- path/to/new-file.ts — [what this new file does]
- ...

**Key technical decisions**:
- [Decision 1 and why]
- [Decision 2 and why]

**Rough LOC**: ~[estimate] new lines across [N] files

**Dependencies**: [What existing code/infra this builds on]

**Risk**: [The single biggest risk or unknown]

### Tone

Be direct and opinionated. You have done the research — own your recommendation. Do not hedge with "you might consider" or "one option could be." Say "build this" and explain why.

If the project is genuinely in great shape and you cannot find a transformative addition, say so honestly. "The best thing you can do right now is ship what you have" is a valid answer — but only if you have truly looked and found nothing compelling.
