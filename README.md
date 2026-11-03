# my-senior-dev

A skill that turns your AI coding assistant into a pragmatic senior engineer for any project. Give it a brief, and it returns concerns, a phased to-do plan, and realistic deadlines, then guides you through the work step by step.

Works for personal side projects and real work tasks. Harness-agnostic: it is a single `SKILL.md` that uses whatever tools your agent has.

## What it does

Run `/my-senior-dev` with a short description of what you want to build, improve, or fix:

```
/my-senior-dev a CLI that syncs my notes to S3, started with uv init
/my-senior-dev add rate limiting to our billing API before the Dec 15 release
/my-senior-dev fix flaky login tests
```

It then:

1. **Recons cheaply** (existing projects): file tree, README, manifest, recent git log, tests/CI presence. It never reads the whole codebase.
2. **Classifies** the work (greenfield, feature, fix, refactor, hardening) and the context (personal or real/work), and scales the process to match.
3. **Asks at most 3 blocking questions**, otherwise states its assumptions and moves on.
4. **Delivers a structured plan**:
   - **Read**: what it understood, plus assumptions
   - **Concerns**: up to 5 ranked risks with a recommended action
   - **Plan**: phased tasks (Foundation, Walking skeleton, Core features, Hardening, Ship), each with a *done when* and an estimate
   - **Timeline**: milestone dates computed from today's date and your pace
   - **Definition of done**: 3-6 testable bullets
   - **Next action**: one concrete step to take right now
5. **Persists the plan** to `SENIOR_PLAN.md` on request. Later invocations read it, report progress, flag slipped dates, and re-plan only what changed.

## Design principles

- **Token-efficient**: terse output, detail only for the current phase, no generic advice.
- **Opinionated**: recommends one option, pushes back on weak ideas, challenges overkill tech, and names what to cut when a deadline doesn't fit.
- **Context-aware**: personal projects stay lightweight. Real work gets a 1.5x estimate buffer, security checks, rollback plans, and docs.
- **Guides, doesn't take over**: it doesn't write implementation code unless you ask, and then works one task at a time against its "done when".

## Installation

Copy `SKILL.md` into your agent's skills directory as `my-senior-dev/SKILL.md`.

| Harness | Location |
|---|---|
| Claude Code (personal) | `~/.claude/skills/my-senior-dev/SKILL.md` |
| Claude Code (project) | `.claude/skills/my-senior-dev/SKILL.md` |
| Other harnesses | Wherever it loads skills or custom instructions from. The file is plain Markdown with YAML frontmatter. |

```bash
mkdir -p ~/.claude/skills/my-senior-dev
cp SKILL.md ~/.claude/skills/my-senior-dev/
```

## Usage notes

- **Explicit only.** The skill never self-triggers (`disable-model-invocation: true`). It runs only when you type `/my-senior-dev`.
- **No brief?** It asks one question (what are you building, and is it personal or for real users?) and stops.
- **Follow-ups.** Run `/my-senior-dev` again in the same repo to review progress against `SENIOR_PLAN.md`, or say "what next".
- **Pace.** It assumes 8 focused hours per week unless you say otherwise. Tell it your real availability for accurate dates.

## Customizing

Edit `SKILL.md` directly. Common tweaks:

- Default pace (hours per week)
- Concern cap (default 5)
- The *Engineering baseline* section, to match your stack, linters, and team conventions
- Plan phases, if your workflow differs

## License

Add your preferred license here (e.g. MIT).
