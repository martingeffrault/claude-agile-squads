# Setup Guide

## Prerequisites

| Requirement | Details |
|-------------|---------|
| **Claude Code Max plan** | Required. Squads spawn multiple agents that run in parallel, consuming significant tokens. Free/Pro tiers will hit rate limits quickly and break the flow. |
| **Claude Code CLI** | Install from [claude.ai/code](https://claude.ai/code) |
| **Web project** | Current flow is designed for web projects (frontend + backend). QA uses Chrome for browser testing. |
| **Chrome** | For QA testing via chrome-devtools MCP |
| **chrome-devtools MCP server** | For browser automation |

> **Note:** This toolkit is optimized for web projects. For CLI tools, APIs without UI, or mobile apps, you'll need to adapt the QA squad (remove Chrome testing, add appropriate testing methods).

## Installation

### 1. Clone or copy to your project

```bash
# Option A: Clone directly into your project
git clone https://github.com/YOUR_USERNAME/claude-agile-squads.git /tmp/agile-squads
cp -r /tmp/agile-squads/skills/ /path/to/your/project/.claude/skills/
cp -r /tmp/agile-squads/qa/ /path/to/your/project/.claude/qa/

# Option B: Copy from local folder
cp -r ~/claude-agile-squads/skills/ /path/to/your/project/.claude/skills/
cp -r ~/claude-agile-squads/qa/ /path/to/your/project/.claude/qa/
```

### 2. Verify structure

Your project should now have:

```
your-project/
├── .claude/
│   ├── skills/
│   │   ├── squad-dev/
│   │   │   └── SKILL.md
│   │   └── squad-test/
│   │       └── SKILL.md
│   └── qa/
│       ├── BACKLOG.md
│       ├── test-plans/
│       ├── bug-reports/
│       ├── test-results/
│       └── screenshots/
└── ... (your project files)
```

### 3. Configure for your project

Edit `.claude/skills/squad-dev/SKILL.md`:

1. **Update MODULES section** (around line 70):
   ```markdown
   | Module | Description | Backend Path | Frontend Path |
   |--------|-------------|--------------|---------------|
   | api | REST API | src/api/ | - |
   | web | React frontend | - | src/web/ |
   | auth | Authentication | src/auth/ | src/web/auth/ |
   ```

2. **Update Tech Leads table** (around line 180):
   ```markdown
   | Agent | Domain | Typical Paths |
   |-------|--------|---------------|
   | @api-lead | API endpoints | src/api/ |
   | @web-lead | React components | src/web/ |
   | @auth-lead | Auth system | src/auth/ |
   ```

### 4. (Optional) Create CLAUDE.md

If your project doesn't have a `CLAUDE.md`, consider creating one with:
- Project overview
- Architecture notes
- Coding standards
- Important patterns

The squads will read this file to understand your project.

## Chrome DevTools MCP Setup (for QA)

### 1. Install the MCP server

Follow instructions at: [chrome-devtools-mcp](https://github.com/anthropics/anthropic-quickstarts/tree/main/mcp-servers/chrome-devtools)

### 2. Configure Claude Code

Add to your Claude Code MCP settings:

```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-chrome-devtools"]
    }
  }
}
```

### 3. Start Chrome with debugging

```bash
# macOS
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222

# Linux
google-chrome --remote-debugging-port=9222

# Windows
"C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222
```

## Usage

### Development Squad

```bash
# Full autonomy - squad analyzes and decides what to build
/squad-dev auto

# With pitch - squad prioritizes specific work
/squad-dev auto "implement user authentication"

# Guided mode - direct execution
/squad-dev "fix the login bug"
```

### QA Squad

```bash
# Test entire application
/squad-test auto

# Test specific module
/squad-test api
/squad-test frontend

# Test cross-module flows only
/squad-test integration
```

## Verification

To verify installation:

1. Open Claude Code in your project directory
2. Type `/squad-dev` - you should see the skill autocomplete
3. Type `/squad-test` - you should see the skill autocomplete

## Troubleshooting

### Skills not appearing

- Check that `.claude/skills/` directory exists
- Verify SKILL.md files have correct frontmatter (--- markers)
- Restart Claude Code

### Chrome MCP not working

- Verify Chrome is running with `--remote-debugging-port=9222`
- Check MCP server is configured in Claude Code settings
- Try `mcp__chrome-devtools__list_pages()` to test connection

### Agents not spawning

- This is expected behavior - only CEO (main agent) can spawn
- Sub-agents send SPAWN REQUESTS instead
- Wait for and execute the SPAWN REQUEST from EM/QA Manager
