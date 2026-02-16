# Claude Agile Squads

> Transform Claude Code into a full Agile development team with autonomous sprints.

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

```bash
# Launch dev squad in full autonomy mode
/squad-dev auto

# Launch dev squad for specific module
/squad-dev auto backend

# Launch QA squad
/squad-test auto

# Test specific module
/squad-test backend
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

### Changing sprint focus

Pass a pitch after `auto`:
```bash
/squad-dev auto "focus on authentication system"
```

The PO will prioritize this in Sprint 1.

## Requirements

- Claude Code CLI
- Chrome + chrome-devtools MCP server (for QA testing)

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
