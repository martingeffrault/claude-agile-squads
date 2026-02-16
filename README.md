# Claude Agile Squads

> Transform Claude Code into a full Agile development team with autonomous sprints.

## Requirements

| Requirement | Why |
|-------------|-----|
| **Claude Code Max plan** | Squads consume significant tokens (multiple agents running in parallel). Free/Pro tiers will hit limits quickly. |
| **Web project** | QA testing uses Chrome DevTools MCP. Adapt for CLI/API projects by removing browser testing. |
| **Chrome + chrome-devtools MCP** | Required for QA squad browser automation. |

## What is this?

A plug-and-play skill pack that gives Claude Code a complete Agile team structure:

- **Product Owner** - Analyzes codebase, creates backlog, prioritizes features
- **Scrum Master** - Facilitates sprints, tracks progress, runs ceremonies
- **Engineering Manager** - Coordinates tech leads, ensures quality
- **Tech Leads** - Actually write the code
- **QA Testers** - Test via Chrome, document bugs

All running autonomously with infinite sprint loops.

## Quick Start

### 1. Copy skills to your project

```bash
cp -r skills/ /path/to/your/project/.claude/skills/
cp -r qa/ /path/to/your/project/.claude/qa/
```

### 2. Configure for your project

Edit `skills/squad-dev/SKILL.md`:
- Update the `MODULES` section with your project's modules
- Update the `Tech Leads disponibles` table
- Adjust paths to match your project structure

### 3. Run

**Two modes available:**

| Mode | Command | What happens |
|------|---------|--------------|
| **Auto** | `/squad-dev auto` | Full Agile structure (PO + SM + EM + Tech Leads). PO analyzes codebase, decides what to build. Infinite sprint loop. |
| **Guided** | `/squad-dev "fix bug"` | No Agile structure. Direct one-shot execution. |

```bash
# AUTO MODE - Full autonomy with Agile structure
/squad-dev auto                              # PO decides everything
/squad-dev auto "focus on auth system"       # PO prioritizes this in Sprint 1, then resumes autonomy

# GUIDED MODE - Direct execution, no PO/SM/EM
/squad-dev "fix the login bug"               # Just does the task

# QA SQUAD
/squad-test auto                             # Test entire app
/squad-test backend                          # Test specific module
```

## Architecture

```
You (CEO/CTO - ONLY one who can spawn agents)
│
├── @product-owner (Task agent)
│   └── Analyzes codebase, creates prioritized backlog (MoSCoW)
│
├── @scrum-master (Task agent)
│   └── Sprint ceremonies, progress tracking, infinite loop
│
├── @engineering-manager (Task agent)
│   └── Coordinates, sends SPAWN REQUESTS to CEO
│
└── Tech Leads (spawned by YOU on EM's request)
    └── Actually write code, report to EM
```

### Why SPAWN REQUESTS?

**Critical constraint:** Sub-agents CANNOT spawn other sub-agents in Claude Code.

Only the main agent (you as CEO) can use the Task tool. So:
1. EM analyzes sprint stories
2. EM sends formatted SPAWN REQUEST to CEO
3. CEO spawns the requested Tech Lead
4. Tech Lead codes and reports back to EM

## Sprint Flow

```
┌─────────────────────────────────────────────────────────────┐
│                      SPRINT LOOP                            │
│                                                             │
│  CEO spawns → PO + SM + EM (once at start)                 │
│                                                             │
│  ┌──────────────┐                                          │
│  │ @product-owner│ Analyzes codebase, creates backlog      │
│  └──────┬───────┘                                          │
│         │ sends backlog                                    │
│         ▼                                                  │
│  ┌──────────────┐                                          │
│  │ @scrum-master│ Sprint Planning, announces start         │
│  └──────┬───────┘                                          │
│         │ triggers                                         │
│         ▼                                                  │
│  ┌──────────────┐                                          │
│  │ @eng-manager │ Analyzes → sends SPAWN REQUESTS to CEO   │
│  └──────┬───────┘                                          │
│         │ spawn requests                                   │
│         ▼                                                  │
│  ┌──────────────┐                                          │
│  │ CEO (you)    │ Spawns requested Tech Leads              │
│  └──────┬───────┘                                          │
│         │                                                  │
│         ▼                                                  │
│  ┌──────────────┐                                          │
│  │ Tech Leads   │ CODE directly, report to EM              │
│  └──────┬───────┘                                          │
│         │ features done                                    │
│         ▼                                                  │
│  ┌──────────────┐                                          │
│  │ @scrum-master│ Sprint Review, Retro                     │
│  └──────┬───────┘                                          │
│         │                                                  │
│         └──────────► LOOP (PO prepares next backlog)       │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

## QA Integration

The QA squad produces bug reports that dev squads consume:

```
.claude/qa/
├── test-plans/          # Test plans per module
├── bug-reports/         # BUG-XXX-[module]-[title].md
├── test-results/        # Execution results
├── screenshots/         # Evidence
└── BACKLOG.md           # Prioritized bugs for dev squads
```

Dev squad POs should check `qa/BACKLOG.md` during Sprint Planning.

## Customization

### Adding a new module

1. Add to the `MODULES` section in `squad-dev/SKILL.md`
2. Add corresponding Tech Lead to the table
3. Update EM prompt with the new domain

### Orienting Sprint 1

In auto mode, you can optionally pass a pitch to orient the first sprint:
```bash
/squad-dev auto "focus on authentication system"
```

The PO will prioritize this pitch in Sprint 1, then resume full autonomy for subsequent sprints.

## Files

```
claude-agile-squads/
├── README.md                    # This file
├── skills/
│   ├── squad-dev/SKILL.md      # Generic dev squad
│   └── squad-test/SKILL.md     # QA/Test squad
├── qa/
│   ├── BACKLOG.md              # Bug backlog template
│   ├── test-plans/
│   ├── bug-reports/
│   ├── test-results/
│   └── screenshots/
└── docs/
    ├── SETUP.md                # Detailed setup guide
    └── CUSTOMIZATION.md        # How to adapt to your project
```

## License

MIT - Use it, fork it, improve it.

## Credits

Built with Claude Code by the Hivanced team.
