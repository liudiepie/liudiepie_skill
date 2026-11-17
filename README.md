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

You can install liudiepie on any computer that has Claude Code. You install it once per computer, not once per project; after that it works in every project on that computer. You don't need to download or clone this repository.

### What you need first

| You need | For | How to check |
| :--- | :--- | :--- |
| Claude Code (the `claude` CLI, or the Code tab in the Claude desktop app) | Every option | Open Claude Code and start a session |
| Node.js 18 or newer | Option A only | `node --version` in a terminal |
| Git | Option C only | `git --version` in a terminal |

If Node.js is missing, install it from [nodejs.org](https://nodejs.org) or with `brew install node` on a Mac, or use Option B, which needs nothing extra.

### Pick one option

| Option | Best for | Commands you get |
| :--- | :--- | :--- |
| **A. `npx skills`** (recommended) | Most people; works like other skill packs such as Matt Pocock's skills | `/setup-liudiepie`, `/liudiepie` |
| **B. Claude Code plugin** | No Node.js, or sharing with a team | `/liudiepie:setup-liudiepie`, `/liudiepie:liudiepie` |
| **C. Manual copy** | Offline, or editing the skills yourself | `/setup-liudiepie`, `/liudiepie` |

Use only one option per computer. If you install more than one, every command shows up twice.

### Option A: Install with `npx skills` (recommended)

In a terminal (not inside Claude Code), run:

```bash
npx skills add liudiepie/liudiepie_skill
```

Follow the prompts. When it asks where to install, choose Claude Code and a global (user-level) install, so the skills work in every project.

### Option B: Install as a Claude Code plugin

This repository is also a Claude Code plugin marketplace. Inside a Claude Code session, run these two commands one at a time:

```text
/plugin marketplace add liudiepie/liudiepie_skill
```

```text
/plugin install liudiepie@liudiepie
```

The first command registers the marketplace under the name `liudiepie`. The second installs the plugin named `liudiepie` from it, which is why it reads `liudiepie@liudiepie`. You can also run `/plugin` by itself and pick **liudiepie** from the menu.

If you prefer a terminal, the same steps are:

```bash
claude plugin marketplace add liudiepie/liudiepie_skill
```

```bash
claude plugin install liudiepie@liudiepie
```

### Option C: Copy the skills by hand

```bash
git clone https://github.com/liudiepie/liudiepie_skill.git
```

```bash
mkdir -p ~/.claude/skills && cp -R liudiepie_skill/skills/liudiepie liudiepie_skill/skills/setup-liudiepie ~/.claude/skills/
```

To install for a single project instead, copy the two folders into that project's `.claude/skills/` folder.

### Check that it worked

1. **Start a new Claude Code session.** Sessions that were already open before you installed won't see the new commands.
2. Type `/setup`. You should see `/setup-liudiepie` (or `/liudiepie:setup-liudiepie` with Option B) in the list.

If it doesn't appear:

- **Options A and C:** check that the skill file exists:
  ```bash
  ls ~/.claude/skills/setup-liudiepie/SKILL.md
  ```
  If it's missing, the install didn't finish; run it again.
- **Option B:** run `/plugin` and make sure `liudiepie` shows as installed and enabled. Remember that plugin commands start with `liudiepie:`.
- **Any option:** make sure you're typing it in Claude Code, not in a regular terminal.

### Update or remove

| Option | Update | Remove |
| :--- | :--- | :--- |
| A | Run `npx skills add liudiepie/liudiepie_skill` again | Delete `~/.claude/skills/liudiepie` and `~/.claude/skills/setup-liudiepie` |
| B | `/plugin marketplace update liudiepie` | `/plugin uninstall liudiepie@liudiepie` |
| C | `git pull` in your clone, then repeat the copy command | Delete `~/.claude/skills/liudiepie` and `~/.claude/skills/setup-liudiepie` |

Start a new session after updating.

## Usage

### 1. Set up each project once

Open Claude Code in a project where you want to use the workflow, and run:

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

With Option B, type `/liudiepie:setup-liudiepie` and `/liudiepie:liudiepie` instead.

## For teams

To give everyone who opens a project the plugin automatically, add this to the project's `.claude/settings.json` and commit it:

```json
{
  "extraKnownMarketplaces": {
    "liudiepie": {
      "source": {
        "source": "github",
        "repo": "liudiepie/liudiepie_skill"
      }
    }
  },
  "enabledPlugins": {
    "liudiepie@liudiepie": true
  }
}
```

Teammates are asked to trust the marketplace the first time they open the project.

## For contributors

To try changes before pushing, install the plugin from your local clone:

```text
/plugin marketplace add ./path/to/liudiepie_skill
```

```text
/plugin install liudiepie@liudiepie
```

When you release a change, raise `version` in both `.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json` so plugin users get the update.
