# Project Tracker

> Fichier centralisé pour le tracking des sprints et de l'avancement.
> **TOUS les squads doivent lire ce fichier au démarrage et le mettre à jour.**

---

## Active Session

> ⚠️ **THIS SECTION IS CRITICAL** - Reread at EVERY sprint, even after context compression.

| Parameter | Value |
|-----------|-------|
| **Mode** | `normal` |
| **Active squad** | - |
| **Current sprint** | - |
| **Started** | - |

### Session Rules
- If `Mode = infinite` → **NEVER** conclude "done". Always start a new sprint.
- If `Mode = normal` → May conclude when backlog empty and module complete.

### How to update
When starting a squad:
```
Mode: infinite (or normal)
Active squad: squad-dev (or your squad name)
Current sprint: Sprint X
Started: YYYY-MM-DD HH:MM
```

### Active Tech Leads this sprint
| Agent | Assigned stories | Status |
|-------|-----------------|--------|
| - | - | - |

### Current Blockers
| Blocker | Since | Owner |
|---------|-------|-------|
| - | - | - |

---

## Critical Rules (REREAD after compression)

> ⚠️ These rules are extracted from skills and MUST be respected even after compression.

### Spawning Constraint
- **ONLY the CEO (main agent) can spawn** agents via Task tool
- Sub-agents (PO, SM, EM, Tech Leads) **CANNOT** spawn
- EM sends **SPAWN REQUESTS** to CEO who executes

### Team Structure
```
CEO (spawns) → PO + SM + EM (management, persistent)
CEO (spawns) → Tech Leads (on EM request, re-spawned each sprint)
```

### SPAWN REQUEST Format (EM → CEO)
```
SPAWN REQUEST:
Agent: @[domain]-lead
Prompt: """
You are the [DOMAIN] TECH LEAD.
Stories: [list]
Paths: [backend + frontend]
You CODE DIRECTLY. Report to EM.
"""
```

### Sprint Flow
1. PO → analysis/backlog → CEO
2. SM → sprint planning → announce start
3. EM → SPAWN REQUESTS → CEO
4. CEO → spawns Tech Leads
5. Tech Leads → code → report to EM
6. SM → review → loop (if infinite) or end (if normal + complete)

### Roles
| Agent | Spawns? | Codes? | Main role |
|-------|---------|--------|-----------|
| CEO | ✅ | ❌ | Supervise, spawn on request |
| PO | ❌ | ❌ | Backlog, priorities, acceptance |
| SM | ❌ | ❌ | Facilitate sprints, track progress |
| EM | ❌ | ❌ | Coordinate tech, send spawn requests |
| Tech Lead | ❌ | ✅ | **Code directly**, report to EM |

---

## État Global

| Module | Completion | Dernier Sprint | Velocity | Status |
|--------|------------|----------------|----------|--------|
| backend | -% | - | - | 🔄 À analyser |
| frontend | -% | - | - | 🔄 À analyser |
| api | -% | - | - | 🔄 À analyser |

**Dernière mise à jour:** [DATE]

---

## Sprint Actuel

### Sprint Info
| Champ | Valeur |
|-------|--------|
| **Sprint** | - |
| **Goal** | - |
| **Dates** | - |
| **Squad actif** | - |

### Stories en cours

| ID | Module | Story | SP | Assigné | Status |
|----|--------|-------|----|---------|--------|
| - | - | Aucun sprint actif | - | - | - |

---

## Backlog Priorisé

### P0 - Critical (Sprint suivant)

| ID | Module | Story | SP | Notes |
|----|--------|-------|----|-------|
| - | - | À définir par le PO | - | - |

### P1 - High (2-3 sprints)

| ID | Module | Story | SP | Notes |
|----|--------|-------|----|-------|
| - | - | À définir par le PO | - | - |

### P2 - Medium (Backlog)

| ID | Module | Story | SP | Notes |
|----|--------|-------|----|-------|
| - | - | - | - | - |

### P3 - Low (Nice to have)

| ID | Module | Story | SP | Notes |
|----|--------|-------|----|-------|
| - | - | - | - | - |

---

## Historique des Sprints

### [Module] Squad
| Sprint | SP | Highlights |
|--------|----|-----------|
| - | - | Aucun sprint complété |

---

## Velocity Tracking

| Squad | Sprints | Total SP | Avg SP/Sprint |
|-------|---------|----------|---------------|
| - | 0 | 0 | - |

---

## Bugs Ouverts (depuis QA)

> Voir `.claude/qa/BACKLOG.md` pour la liste complète.

| Priority | Count | Oldest |
|----------|-------|--------|
| P1 Critical | 0 | - |
| P2 Major | 0 | - |
| P3 Minor | 0 | - |

---

## Instructions for Squads

### When starting a squad
1. **READ this file first** (especially "Active Session")
2. **UPDATE "Active Session"** with mode (infinite/normal), squad, date
3. Check the relevant module state
4. Resume existing backlog (don't rescan if already analyzed)
5. Create sprint from P0/P1 backlog

### At EVERY sprint start (IMPORTANT for post-compression survival)
1. **REREAD the "Active Session" section**
2. Check the Mode (infinite/normal)
3. Apply the mode rules

### During sprint
1. Update story status (To Do → In Progress → Done)
2. Log blockers

### At sprint end
1. **REREAD the "Active Session" section** (mode may have been forgotten after compression)
2. Move Done stories to history
3. Update module % completion
4. Update velocity
5. **IF mode=infinite:** ALWAYS prepare next backlog
6. **IF mode=normal:** Prepare next backlog OR conclude if complete

---

*This file is the source of truth for project state.*
