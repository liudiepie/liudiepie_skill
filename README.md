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

This repository is a Claude Code plugin marketplace that contains one plugin, `liudiepie`.

#### Step 1: Add the marketplace

Start Claude Code (`claude` in a terminal, or the Code tab in the Claude desktop app) and run:

```text
/plugin marketplace add <your-github-username>/liudiepie_skill
```

This registers the marketplace under the name `liudiepie`.

#### Step 2: Install the plugin

```text
/plugin install liudiepie@liudiepie
```

The format is `<plugin-name>@<marketplace-name>`. You can also run `/plugin` by itself and pick **liudiepie** from the interactive menu.

#### Step 3: Restart and check

Restart Claude Code so the skill loads, then run `/plugin` and confirm `liudiepie` is listed as installed and enabled.

#### Install from the command line instead

The same steps work outside a Claude Code session:

```bash
claude plugin marketplace add <your-github-username>/liudiepie_skill
```

```bash
claude plugin install liudiepie@liudiepie
```

#### Set it up for a whole team

To have everyone who opens your project get the plugin automatically, add this to the project's `.claude/settings.json` and commit it:

```json
{
  "extraKnownMarketplaces": {
    "liudiepie": {
      "source": {
        "source": "github",
        "repo": "<your-github-username>/liudiepie_skill"
      }
    }
  },
  "enabledPlugins": {
    "liudiepie@liudiepie": true
  }
}
```

Teammates are asked to trust the marketplace the first time they open the project.

#### Update or remove

```text
/plugin marketplace update liudiepie
/plugin uninstall liudiepie@liudiepie
/plugin marketplace remove liudiepie
```

#### Test a local copy

To try changes before pushing, point Claude Code at your local clone:

```text
/plugin marketplace add ./path/to/liudiepie_skill
/plugin install liudiepie@liudiepie
```

### Option 2: Install via `npx skills`

```bash
npx skills add <your-github-username>/liudiepie_skill
```

### Option 3: Manual Skill Installation

Copy both skill folders into your global (`~/.claude/skills`) or project (`.claude/skills`) skills folder:

```bash
mkdir -p ~/.claude/skills
cp -R skills/liudiepie skills/setup-liudiepie ~/.claude/skills/
```

## Usage

### 1. Set up each project once

In every project where you want to use the workflow, run this first:

```text
/setup-liudiepie
```

It finds the project's typecheck, lint, test, and run commands, asks you to confirm them, saves them to a `## liudiepie verification` section in the project's `CLAUDE.md`, and runs each one once to show the starting state. The workflow's verification phase then uses exactly those commands. You can add notes, for example `/setup-liudiepie monorepo, only packages/api`.

Run it again whenever those commands change. Claude never starts setup on its own; it only runs when you type the command.

### 2. Use the workflow

Invoke the skill explicitly or let Claude trigger it automatically on multi-step coding tasks:

```text
/liudiepie add webhook signature verification to our /api/events endpoint
```

### Command names when installed as a plugin

Claude Code puts the plugin name in front of plugin skills, so with Option 1 the commands are:

| Command | Option 1 (plugin) | Options 2 and 3 |
| :--- | :--- | :--- |
| Setup | `/liudiepie:setup-liudiepie` | `/setup-liudiepie` |
| Workflow | `/liudiepie:liudiepie` | `/liudiepie` |

In either case you can type `/setup` and pick the command from the autocomplete list.
