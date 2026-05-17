# Karpathy-Inspired Claude Code Guidelines (Expanded)

A single `SKILL.md` file to improve Claude Code behavior, derived from [Andrej Karpathy's observations](https://x.com/karpathy/status/2015883857489522876) on LLM coding pitfalls, then expanded with additional rules.

English | [简体中文](./README.zh.md)

## The Problems

From Andrej's post:

> "The models make wrong assumptions on your behalf and just run along with them without checking. They don't manage their confusion, don't seek clarifications, don't surface inconsistencies, don't present tradeoffs, don't push back when they should."

> "They really like to overcomplicate code and APIs, bloat abstractions, don't clean up dead code... implement a bloated construction over 1000 lines when 100 would do."

> "They still sometimes change/remove comments and code they don't sufficiently understand as side effects, even if orthogonal to the task."

## The Solution

Twelve principles in one file that directly address these issues and more:

| Rule | Addresses |
|------|-----------|
| **Think Before Coding** | Wrong assumptions, hidden confusion, missing tradeoffs |
| **Simplicity First** | Overcomplication, bloated abstractions |
| **Surgical Changes** | Orthogonal edits, touching code you shouldn't |
| **Goal-Driven Execution** | Leverage through tests-first, verifiable success criteria |
| **Use Model for Judgment** | Offloading deterministic logic to LLMs |
| **Token Budgets** | Silent context overruns |
| **Surface Conflicts** | Averaging contradictory patterns |
| **Read Before Write** | Adding code without understanding callers |
| **Tests Verify Intent** | Tests that don't encode business meaning |
| **Checkpoint** | Losing track of state mid-task |
| **Match Conventions** | Silently forking codebase style |
| **Fail Loud** | Suppressing errors and skipped checks |

## The Twelve Rules in Detail

### Rule 1 — Think Before Coding

State assumptions explicitly. If uncertain, ask rather than guess.
Present multiple interpretations when ambiguity exists.
Push back when a simpler approach exists.
Stop when confused. Name what's unclear.

### Rule 2 — Simplicity First

Minimum code that solves the problem. Nothing speculative.
No features beyond what was asked. No abstractions for single-use code.
Test: would a senior engineer say this is overcomplicated? If yes, simplify.

### Rule 3 — Surgical Changes

Touch only what you must. Clean up only your own mess.
Don't "improve" adjacent code, comments, or formatting.
Don't refactor what isn't broken. Match existing style.

### Rule 4 — Goal-Driven Execution

Define success criteria. Loop until verified.
Don't follow steps. Define success and iterate.
Strong success criteria let you loop independently.

### Rule 5 — Use the Model Only for Judgment Calls

Use the model for: classification, drafting, summarization, extraction.
Do NOT use it for: routing, retries, deterministic transforms.
If code can answer, code answers.

### Rule 6 — Token Budgets Are Not Advisory

Per-task: 4,000 tokens. Per-session: 30,000 tokens.
If approaching budget, summarize and start fresh.
Surface the breach. Do not silently overrun.

### Rule 7 — Surface Conflicts, Don't Average Them

If two patterns contradict, pick one (more recent / more tested).
Explain why. Flag the other for cleanup.
Don't blend conflicting patterns.

### Rule 8 — Read Before You Write

Before adding code, read exports, immediate callers, shared utilities.
"Looks orthogonal" is dangerous. If unsure why code is structured a way, ask.

### Rule 9 — Tests Verify Intent, Not Just Behavior

Tests must encode WHY behavior matters, not just WHAT it does.
A test that can't fail when business logic changes is wrong.

### Rule 10 — Checkpoint After Every Significant Step

Summarize what was done, what's verified, what's left.
Don't continue from a state you can't describe back.
If you lose track, stop and restate.

### Rule 11 — Match the Codebase's Conventions, Even If You Disagree

Conformance > taste inside the codebase.
If you genuinely think a convention is harmful, surface it. Don't fork silently.

### Rule 12 — Fail Loud

"Completed" is wrong if anything was skipped silently.
"Tests pass" is wrong if any were skipped.
Default to surfacing uncertainty, not hiding it.

## Install

**Option A: Claude Code Plugin (recommended)**

From within Claude Code, first add the marketplace:
```
/plugin marketplace add onemott/andrej-karpathy-skills
```

Then install the plugin:
```
/plugin install andrej-karpathy-skills@karpathy-skills
```

**Option B: CLAUDE.md (per-project)**

New project:
```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/onemott/andrej-karpathy-skills/main/CLAUDE.md
```

Existing project (append):
```bash
echo "" >> CLAUDE.md
curl https://raw.githubusercontent.com/onemott/andrej-karpathy-skills/main/CLAUDE.md >> CLAUDE.md
```

## Using with Cursor

This repository includes a committed Cursor project rule ([`.cursor/rules/karpathy-guidelines.mdc`](.cursor/rules/karpathy-guidelines.mdc)) so the same guidelines apply when you open the project in Cursor. See **[CURSOR.md](CURSOR.md)** for setup, using the rule in other projects, and how this relates to Claude Code.

## Key Insight

From Andrej:

> "LLMs are exceptionally good at looping until they meet specific goals... Don't tell it what to do, give it success criteria and watch it go."

The "Goal-Driven Execution" principle captures this: transform imperative instructions into declarative goals with verification loops.

## How to Know It's Working

These guidelines are working if you see:

- **Fewer unnecessary changes in diffs** — Only requested changes appear
- **Fewer rewrites due to overcomplication** — Code is simple the first time
- **Clarifying questions come before implementation** — Not after mistakes
- **Clean, minimal PRs** — No drive-by refactoring or "improvements"
- **Explicit tradeoffs surfaced** — Conflicting patterns flagged, not hidden
- **Budget awareness** — Token limits respected, not silently exceeded

## Customization

These guidelines are designed to be merged with project-specific instructions. Add them to your existing `CLAUDE.md` or create a new one.

For project-specific rules, add sections like:

```markdown
## Project-Specific Guidelines

- Use TypeScript strict mode
- All API endpoints must have tests
- Follow the existing error handling patterns in `src/utils/errors.ts`
```

## Tradeoff Note

These guidelines bias toward **caution over speed**. For trivial tasks (simple typo fixes, obvious one-liners), use judgment — not every change needs the full rigor.

The goal is reducing costly mistakes on non-trivial work, not slowing down simple tasks.

## License

MIT
