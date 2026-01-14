# devis

> Interviewing to understand requirements, and then implementing them using a Manus-style approach.

`devis` is a Claude Code plugin that understands requirements through structured interview workflows, then implements features based on Manus's context engineering principles. It uses persistent Markdown files as "working memory on disk" to ensure goals and progress are never lost during complex tasks.

## Key Features

- **Structured Interviewing** (`/devis:intv`) - Clarify requirements, technical solutions, and trade-offs through in-depth interviews
- **Progressive Implementation** (`/devis:impl`) - Manus-style workflow using the filesystem as external memory
- **Context Engineering** - Based on Manus AI agent best practices, generates three core files: `task_plan.md`, `findings.md`, `progress.md`
- **Git Worktree Support** - Develop safely in isolated working trees
- **Context Engineering** - Built on production-grade AI agent best practices from pre-acquisition Manus

## Installation

### Method 1: Claude Code Marketplace (Recommended)

Run in Claude Code:

```bash
/plugin marketplace add st01cs/devis
/plugin install devis@devis
```

### Method 2: Manual Installation

```bash
# Clone repository to Claude Code plugins directory
git clone https://github.com/st01cs/devis.git ~/.claude/plugins/devis
```

## Usage

### Workflow

devis adopts a two-stage workflow:

```
User Requirements → /devis:intv (Interview) → Planning Docs → /devis:impl (Implementation) → Final Delivery
```

### Step 1: Interview & Plan

Use the `/devis:intv` command for requirements interview:

```bash
/devis:intv path/to/your/plan.md
```

**Interview Process:**

- Claude will ask in-depth questions about technical implementation, UI/UX, concerns, trade-offs, etc.
- After the interview, three files are automatically generated:
  - `task_plan.md` - Phase breakdown, progress tracking, decision records
  - `findings.md` - Research findings, interview content, technical decisions
  - `progress.md` - Session logs, test results

### Step 2: Implement

Use the `/devis:impl` command to implement according to plan:

```bash
/devis:impl path/to/your/plan.md
```

**Implementation Process:**

- Automatically creates Git Worktree under `.worktrees/`
- Executes plan phase by phase, updating status after each completion
- Records all errors and solutions
- Uses "2-Action Rule" to prevent loss of multimodal information
- Verifies all phases complete upon finish

## File Structure

```
devis/
├── .claude-plugin/
│   ├── plugin.json          # Plugin metadata
│   └── marketplace.json      # Marketplace configuration
├── commands/
│   ├── intv.md              # /intv command definition
│   └── impl.md              # /impl command definition
├── templates/
│   ├── task_plan.md         # Task plan template
│   ├── findings.md          # Findings template
│   └── progress.md          # Progress log template
├── refs/
│   ├── manus.md             # Manus principles reference
│   └── examples.md          # Practical examples
├── scripts/
│   └── check-complete.sh    # Completion check script
└── README.md
```

## Example Scenario

```bash
# 0. Requirements draft
# Create a file dev-docs/plan/feature-xxx/feature-draft.md with a simple description of requirements

# 1. Interview requirements
/devis:intv dev-docs/plan/feature-xxx/feature-draft.md

# Interview will ask: design preferences, state management, compatibility requirements, etc.
# After interview, three files are automatically generated:
# - dev-docs/plan/feature-xxx/task_plan.md
# - dev-docs/plan/feature-xxx/findings.md
# - dev-docs/plan/feature-xxx/progress.md

# 2. Implement feature
/devis:impl dev-docs/plan/feature-xxx/task_plan.md

# Automatically creates worktree, implements by phases, tracks progress
```

## Advanced Usage

### Custom Templates

You can modify files under `templates/` to customize the planning process:

```bash
# Edit template
nano ~/.claude/plugins/devis/templates/task_plan.md
```

## FAQ

### Q: Why do we need Git Worktree?

A: Worktree provides an isolated development environment, avoiding direct modifications on the main branch. All changes are merged back to main branch only after completion.

### Q: How to resume interrupted work?

A: Simply run `/devis:impl path/to/task_plan.md` again, it will read existing files and continue from the current phase.

## Acknowledgments

The core concepts and methodology of this project are deeply inspired by:

- **Manus** - [Context Engineering for AI Agents](https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus)

- **planning-with-files** - [planning-with-files](https://github.com/OthmanAdi/planning-with-files)

## License

MIT License - See [LICENSE](LICENSE) for details

## Author

st01cs - [GitHub](https://github.com/st01cs)
