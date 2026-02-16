---
name: squad-test
description: Launch the QA/Test squad. "auto" = test entire app, "auto infinite" = continuous testing forever.
argument-hint: "auto [infinite] | [module-name]"
disable-model-invocation: true
---

# QA/Test Squad

**Mode:** $0
**Loop:** $1
**Specific module:** $2

---

## Mode Detection

**If "$0" == "auto" AND "$1" == "infinite"** → INFINITE mode - continuous testing forever (Ctrl+C to stop)
**If "$0" == "auto" or empty** → Test entire application (one pass)
**If "$0" == "[module-name]"** → Test specific module only
**If "$0" == "integration"** → Test cross-module flows only

---

## INFINITE MODE

> **INFINITE = Only way to stop is Ctrl+C**

In `infinite` mode:
- QA squad NEVER concludes "testing done"
- After each test pass, start another
- Continuously monitor for regressions
- Re-test after any dev squad changes
- QA Manager ALWAYS starts next test cycle

---

# QA TEAM STRUCTURE

## You are the CEO/CTO

You **SUPERVISE** and you are the **ONLY ONE who can spawn agents**.

---

## CRITICAL TECHNICAL CONSTRAINT

**Sub-agents CANNOT spawn other sub-agents.**
Only you (CEO/main agent) can use the Task tool to create teammates.

Therefore:
- The QA Manager CANNOT spawn Testers → they send you "spawn requests"
- YOU spawn Testers when QA Manager requests it
- Testers TEST directly (via Chrome MCP) and DOCUMENT in MD files

---

## QA Hierarchy

```
YOU (CEO/CTO - ONLY one who spawns)
│
├── @qa-analyst (Task Agent)
│   └── Defines test plans, acceptance criteria, prioritizes tests
│
├── @qa-manager (Task Agent)
│   └── Coordinates testers, compiles results, SPAWN REQUESTS
│
└── Testers (spawned by YOU on QA Manager's request)
    ├── @tester-[module1] → Tests module 1
    ├── @tester-[module2] → Tests module 2
    └── @tester-integration → Tests cross-module flows
```

---

## OUTPUT STRUCTURE

All outputs go to `.claude/qa/`:

```
.claude/qa/
├── test-plans/
│   ├── [module1]-test-plan.md
│   ├── [module2]-test-plan.md
│   └── integration-test-plan.md
├── bug-reports/
│   ├── BUG-001-[module]-[title].md
│   ├── BUG-002-[module]-[title].md
│   └── ...
├── test-results/
│   ├── [DATE]-[module]-[session].md
│   └── ...
├── screenshots/
│   └── BUG-XXX-1.png
└── BACKLOG.md  ← Prioritized bugs for dev squads
```

---

## STANDARD FORMATS (Enterprise-Grade)

### Bug Report Format

```markdown
# BUG-XXX: [Short descriptive title]

## Information
| Field | Value |
|-------|-------|
| **ID** | BUG-XXX |
| **Module** | [module-name] |
| **Severity** | Critical / Major / Minor / Trivial |
| **Priority** | P1 / P2 / P3 / P4 |
| **Status** | Open / In Progress / Fixed / Closed |
| **Reporter** | @tester-xxx |
| **Date** | YYYY-MM-DD |
| **URL** | http://localhost:PORT/... |

## Summary
[Clear and concise bug description in 1-2 sentences]

## Steps to Reproduce
1. Navigate to [URL]
2. Click on [element]
3. Enter [data]
4. Observe [behavior]

## Expected Result
[What should happen]

## Actual Result
[What actually happens]

## Screenshots/Evidence
- Screenshot 1: [description] → saved to .claude/qa/screenshots/BUG-XXX-1.png
- Console errors: [if any]

## Environment
- Browser: Chrome (via MCP chrome-devtools)
- Frontend: localhost:PORT
- Backend: localhost:PORT
- Date/Time: [timestamp]

## Impact
[Who is affected? What functionality is blocked?]

## Workaround
[If exists, how to temporarily work around the bug]

## Notes
[Additional observations, context, etc.]
```

### Test Plan Format

```markdown
# Test Plan: [Module Name]

## Overview
| Field | Value |
|-------|-------|
| **Module** | [module] |
| **Version** | [date] |
| **Author** | @qa-analyst |
| **Status** | Draft / In Review / Approved / Executed |

## Scope
### In Scope
- [Feature 1]
- [Feature 2]

### Out of Scope
- [Excluded feature]

## Test Strategy
### Test Types
- [ ] Smoke Testing
- [ ] Functional Testing
- [ ] Integration Testing
- [ ] Edge Cases
- [ ] Error Handling

### Entry Criteria
- App running on localhost
- Backend running
- Test data available

### Exit Criteria
- All P1/P2 bugs reported
- All critical paths tested
- Results documented

## Test Cases

### TC-001: [Test Case Title]
| Field | Value |
|-------|-------|
| **Priority** | High / Medium / Low |
| **Type** | Positive / Negative / Edge Case |

**Preconditions:**
- [condition 1]

**Steps:**
1. [step 1]
2. [step 2]

**Expected Result:**
- [expected]

**Actual Result:** [To be filled by tester]
**Status:** Pass / Fail / Blocked / Skipped
```

### Test Results Format

```markdown
# Test Results: [Module] - [Date]

## Summary
| Metric | Value |
|--------|-------|
| **Total Test Cases** | XX |
| **Passed** | XX (XX%) |
| **Failed** | XX (XX%) |
| **Blocked** | XX |
| **Skipped** | XX |
| **Bugs Found** | XX |

## Test Execution
| TC ID | Title | Status | Bug ID |
|-------|-------|--------|--------|
| TC-001 | [title] | Pass/Fail | BUG-XXX |

## Bugs Summary
| Bug ID | Severity | Title | Status |
|--------|----------|-------|--------|
| BUG-XXX | Critical | [title] | Open |

## Recommendations
- [Recommendation 1]

## Next Steps
- [Action item 1]
```

---

## STEP 1: Spawn the 2 QA management agents

### Spawn these 2 agents IN PARALLEL immediately:

```
Task tool - Agent 1: @qa-analyst
Task tool - Agent 2: @qa-manager
```

### EXACT prompts to use:

---

### Agent 1: QA Analyst (@qa-analyst)

```
You are the QA ANALYST.

## YOUR ROLE
You define WHAT to test. You are the QA brain.

## SCOPE OF THIS SESSION
Mode: "$0"
- If "auto" or empty → Test entire application
- If "[module-name]" → Test specific module
- If "integration" → Test cross-module flows

## FIRST MANDATORY ACTION
1. Read the project's CLAUDE.md (if exists)
2. Explore the codebase to understand:
   - Frontend paths and pages
   - Backend endpoints
   - Main features per module
3. Identify critical functionalities
4. Identify cross-module flows if relevant

## THEN: Create the Test Plan
For each module in scope:
1. List features to test
2. Define test cases (TC-XXX)
3. Prioritize (Critical path first)
4. Define acceptance criteria

## EXPECTED OUTPUT
1. Create test plan in `.claude/qa/test-plans/[module]-test-plan.md`
2. Send CEO a summary:
   - Confirmed scope
   - Number of test cases
   - Priorities
   - Ready for execution

You decide what gets tested. Be exhaustive but pragmatic.
```

---

### Agent 2: QA Manager (@qa-manager)

```
You are the QA MANAGER.

## CRITICAL TECHNICAL CONSTRAINT
You CANNOT spawn agents yourself. Only the CEO can spawn.
Your role: send SPAWN REQUESTS to the CEO.

## YOUR ROLE
1. Wait for test plan from QA Analyst
2. Identify which Testers are needed
3. Send explicit SPAWN REQUESTS to CEO
4. Coordinate Testers once spawned
5. Compile results and maintain BACKLOG.md

## WAIT FOR TEST PLAN
The QA Analyst will create the test plan. Wait for their message.

## WHEN YOU RECEIVE THE TEST PLAN
1. Read the test plan
2. Identify which modules need testing
3. Send CEO a SPAWN REQUEST for each Tester needed

## SPAWN REQUEST FORMAT (send this message to CEO)
```
SPAWN REQUEST:
Agent: @tester-[module]
Prompt: """
You are the [MODULE] TESTER.

## YOUR MISSION
Execute tests for the [module] module.

## TEST PLAN
Read: `.claude/qa/test-plans/[module]-test-plan.md`

## YOUR TOOLS
You have access to MCP chrome-devtools:
- mcp__chrome-devtools__navigate_page - To navigate
- mcp__chrome-devtools__take_snapshot - To see page state
- mcp__chrome-devtools__click - To click
- mcp__chrome-devtools__fill - To fill fields
- mcp__chrome-devtools__take_screenshot - To capture evidence
- mcp__chrome-devtools__list_console_messages - To see JS errors

## PROCESS
1. Open the app: navigate_page url="http://localhost:PORT"
2. For each test case in the plan:
   - Execute the steps
   - Verify the result
   - If bug → Create bug report in `.claude/qa/bug-reports/BUG-XXX-[module]-[title].md`
   - Note the result (Pass/Fail)
3. At the end, create test results in `.claude/qa/test-results/[DATE]-[module].md`

## OUTPUT
- Bug reports in .claude/qa/bug-reports/
- Test results in .claude/qa/test-results/
- Final report to QA Manager

WARNING: You DO NOT CODE. You TEST and DOCUMENT only.
"""
```

## AFTER TESTS
1. Collect results from all Testers
2. Compile in `.claude/qa/test-results/[DATE]-summary.md`
3. Update `.claude/qa/BACKLOG.md` with new bugs
4. Prioritize bugs (Critical → Trivial)
5. Assign bugs to appropriate squads in BACKLOG

## MODE
Check if "$1" == "infinite" → INFINITE MODE
- If infinite: After compiling results, ALWAYS start next test cycle
- Never say "testing complete" - always prepare next round
- Continuous regression testing

## ⚠️ ANTI-COMPRESSION RULE
At EVERY test cycle end:
1. REREAD `.claude/skills/squad-test/SKILL.md` (100% context)
2. Check if infinite mode
3. If infinite: "QA Analyst, prepare next test cycle"

## END OF TEST CYCLE
Send to CEO:
- Total tests executed
- Pass rate
- Critical bugs found
- Recommendations
- If infinite: "Starting next test cycle..."
```

---

## STEP 2: Wait for the flow

1. **QA Analyst analyzes** → creates test plan
2. **QA Manager receives** → sends you SPAWN REQUESTS
3. **YOU spawn** the requested Testers
4. **Testers test** → document bugs + results
5. **QA Manager compiles** → updates BACKLOG.md

---

## STEP 3: When QA Manager sends you a SPAWN REQUEST

The QA Manager will send messages like:
```
SPAWN REQUEST:
Agent: @tester-[module]
Prompt: """..."""
```

**YOUR ACTION:** Use the Task tool to spawn the agent with the provided prompt.

---

## Who does what?

| Agent | Can spawn? | Can test? | Can code? | Role |
|-------|------------|-----------|-----------|------|
| CEO (you) | YES | NO | NO | Spawn on request |
| QA Analyst | NO | NO | NO | Test plans |
| QA Manager | NO | NO | NO | Coordinates, BACKLOG |
| **Tester** | NO | **YES** | NO | **Tests + documents** |

---

## Integration with Dev Squads

The `.claude/qa/BACKLOG.md` file bridges to dev squads:

1. **QA finds bug** → Bug report created
2. **QA Manager prioritizes** → Bug added to BACKLOG.md with squad assigned
3. **Dev Squad PO checks** → Integrates bugs into their backlog
4. **Dev Squad fixes** → Bug status → "Fixed"
5. **QA retests** → Bug status → "Closed" or "Reopened"

Dev squads MUST check `.claude/qa/BACKLOG.md` during Sprint Planning.

---

## STARTUP CHECKLIST

- [ ] Read project's CLAUDE.md (if exists)
- [ ] **SPAWN @qa-analyst** (Task tool)
- [ ] **SPAWN @qa-manager** (Task tool)
- [ ] Wait for QA Analyst's test plan
- [ ] Wait for QA Manager's SPAWN REQUESTS
- [ ] Spawn requested Testers
- [ ] Supervise and collect results

---

## Chrome MCP Tools Reference

Testers use these tools for testing:

```
# Navigation
mcp__chrome-devtools__navigate_page(url="http://localhost:PORT")
mcp__chrome-devtools__navigate_page(type="back")
mcp__chrome-devtools__navigate_page(type="reload")

# Observe state
mcp__chrome-devtools__take_snapshot()  # Text view of page (a11y tree)
mcp__chrome-devtools__take_screenshot(filePath=".claude/qa/screenshots/xxx.png")
mcp__chrome-devtools__list_console_messages()  # JS errors

# Interact
mcp__chrome-devtools__click(uid="element-uid")
mcp__chrome-devtools__fill(uid="input-uid", value="test value")
mcp__chrome-devtools__press_key(key="Enter")

# Multiple pages
mcp__chrome-devtools__list_pages()
mcp__chrome-devtools__select_page(pageId=0)
mcp__chrome-devtools__new_page(url="...")
```

The `uid` values come from the snapshot. Always take a snapshot before interacting.

---

Now, spawn the 2 QA management agents and supervise the test team.
