# liudiepie

A high-efficiency 4-phase engineering workflow skill and plugin for Claude Code. It prevents wasted tokens on misunderstood requirements, eliminates AI code slop, and proves correctness with runtime verification evidence.

## Workflow Phases

| Phase | Core Practice | Payoff |
| :--- | :--- | :--- |
| **1. Grill & Align** | Reads entry points first, then asks up to 5 numbered questions with recommended defaults (with a 2-line fast-track escape hatch for trivial tasks). | Eliminates wrong-assumption rewrites before tokens are spent coding. |
| **2. Investigate & Scope** | Traces real call chains, reuses existing helpers, reproduces bugs first, and keeps diffs strictly surgical. | Prevents duplicate utilities and speculative over-engineering. |
| **3. Build with Constraints** | Writes failing tests first, encodes invariants in types/linters instead of prose or narrative comments, and keeps modules focused. | Makes regressions mechanically impossible instead of relying on fragile prompt reminders. |
| **4. Verify with Evidence** | Runs typecheck/lint/tests, executes the real CLI/API/UI path, and performs a 2-pass self-review (Intent pass + Slop removal pass). | Delivers concrete before/after proof instead of unverified diffs. |

## Installation

### Option 1: Install as a Claude Code Plugin (Recommended)

Inside Claude Code, add this GitHub repository as a marketplace and install the plugin:

```text
/plugin marketplace add <your-github-username>/liudiepie_skill
/plugin install liudiepie@liudiepie
```

### Option 2: Install via `npx skills`

```bash
npx skills add <your-github-username>/liudiepie_skill
```

### Option 3: Manual Skill Installation

Copy `skills/liudiepie/SKILL.md` into your global or project skills folder:

```bash
mkdir -p ~/.claude/skills/liudiepie
cp skills/liudiepie/SKILL.md ~/.claude/skills/liudiepie/SKILL.md
```

## Usage

Invoke the skill explicitly or let Claude trigger it automatically on multi-step coding tasks:

```text
/liudiepie add webhook signature verification to our /api/events endpoint
```
