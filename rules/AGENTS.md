# 🤖 AGENTS

> **LOAD**: When delegating to agents | **PURPOSE**: Agent handling protocol

---

## TIERED EXECUTION

### TIER 1: Sub-agent (MANDATORY when tool exists)

```yaml
1. Prepare handoff:
   include: requirements, task, acceptance criteria, constraints
   exclude: internal reasoning, failed attempts

2. Skills analysis: (output required)
   "🎯 Skills Analysis: {simple|complex} → {using X | skipping}"

3. Invoke: runSubagent(agent_name, context)

4. Verify: format matches, criteria met

5. On error: fallback to TIER 2, log reason
```

### TIER 2: EMBODY (Fallback only)

```yaml
permitted_when:
  - Tool Discovery found NO sub-agent tools
  - Sub-agent tool returned system error

forbidden_reasons:
  - Task seems "simple"
  - "Save tokens"
  - "Efficiency"

execution:
  1. Log: "⚠️ TIER 2: {reason}"
  2. READ agent file COMPLETELY
  3. EXTRACT: Directive, Protocol, Constraints, Format
  4. ANNOUNCE embodiment (see format below)
  5. EXECUTE as agent (follow THEIR protocol)
  6. EXIT embodiment, continue as orchestrator
```

**Embodiment Announcement Format**:
```markdown
📋 EMBODIED: `{agent}`
**Directive**: {core directive verbatim}
**Protocol**: {thinking protocol summary}
**Constraints**: {key constraints}
```

---

## TOOL DISCOVERY (First delegation only)

```markdown
## 🔍 Tool Discovery
| Check | Result |
|-------|--------|
| Sub-agent tool | ✅ / ❌ |
| Execution tier | TIER 1 / TIER 2 |
```

**Cache**: Tool discovery result is cached for session. Do not re-check.

---

## CONTEXT MODEL COMPARISON

| Aspect | TIER 1: Sub-agent | TIER 2: EMBODY |
|--------|-------------------|----------------|
| Priority | ⭐ MANDATORY | 🔄 Fallback |
| Context | Fresh, isolated | Shared with parent |
| Quality | ✅ Optimal | ⚠️ Risk of pollution |
| Parallel | Yes | No (sequential) |
| Availability | Platform-dependent | Always available |

---

## COMPLETION GUARANTEE

```yaml
rule: "EVERY delegation request WILL be fulfilled"

mechanism:
  - TIER 1 is primary when available
  - TIER 2 is fallback when TIER 1 fails
  - EMBODY always works (read + transform)

result:
  - NO task is ever skipped
  - NO delegation ever fails completely
  - System is future-proof
```

---

## AGENT CATEGORIES

| Category | Agents | Purpose |
|----------|--------|---------|
| **meta** | tech-lead, planner, wiki-architect | Coordinate, plan — never implement |
| **execution** | backend-engineer, frontend-engineer, mobile-engineer, game-engineer, database-architect | Implementation |
| **validation** | tester, reviewer, security-engineer, performance-engineer, debugger, wiki-reviewer | QA |
| **research** | researcher, scouter, brainstormer, designer, wiki-extractor | Investigation |
| **support** | docs-manager, devops-engineer, business-analyst, project-manager, reporter | Support |

---

## 🔺 AGENT TEAMS — GOLDEN TRIANGLE (`:team` variant only)

> **LOAD**: `TEAMS.md` for full Golden Triangle protocol and debate mechanism.
> Teams spawn exactly **3 agents per phase**: Tech Lead + Executor + Reviewer.
> Adversarial collaboration produces higher quality than parallel cooperation.

### Golden Triangle Roster

| Domain | Tech Lead | Executor | Reviewer | Use When |
|--------|-----------|----------|----------|----------|
| `backend-team` | `tech-lead` | `backend-engineer` | `reviewer` | APIs, server logic, backend features |
| `frontend-team` | `tech-lead` | `frontend-engineer` | `reviewer` | UI components, client-side features |
| `fullstack-team` | `tech-lead` | `backend-engineer` + `frontend-engineer` | `reviewer` | End-to-end features |
| `database-team` | `tech-lead` | `database-architect` | `reviewer` + security lens | Schema design, migrations, queries |
| `research-team` | `researcher` | `scouter` | `brainstormer` (Devil's Advocate) | Discovery, codebase analysis, patterns |
| `planning-team` | `planner` | `researcher` | `tech-lead` (feasibility critic) | Architecture planning, task decomposition |
| `qa-team` | `tester` | `tester` (self-implements) | `security-engineer` + `performance-engineer` | Test strategy, coverage, quality |
| `design-team` | `designer` | `frontend-engineer` | `reviewer` + UX/a11y lens | UI/UX design, component specs |
| `debug-team` | `debugger` | `backend-engineer` | `reviewer` (root-cause validator) | Root cause analysis, issue resolution |
| `devops-team` | `devops-engineer` | `backend-engineer` | `security-engineer` | CI/CD, infrastructure, deployment |
| `security-team` | `security-engineer` | `backend-engineer` | `reviewer` (pen-test mindset) | Security assessment, vulnerability audit |
| `game-team` | `tech-lead` | `game-engineer` | `reviewer` (game arch + 60fps) | Game development, engines, physics, ECS |
| `mobile-team` | `tech-lead` | `mobile-engineer` | `reviewer` (UX + platform) | iOS, Android, React Native, Flutter |
| `performance-team` | `performance-engineer` | `backend-engineer` | `reviewer` (measurement + regression) | Profiling, optimization, load testing |
| `docs-team` | `docs-manager` | `researcher` | `reviewer` (accuracy + completeness) | Technical writing, API docs, architecture docs |
| `project-team` | `project-manager` | `business-analyst` | `tech-lead` (feasibility critic) | Project planning, risk, delivery |
| `report-team` | `reporter` | `scouter` | `reviewer` (data accuracy + insight) | Status reports, metrics, analytics |
| `wiki-team` | `wiki-architect` | `wiki-extractor` | `wiki-reviewer` | Wiki generation, entity extraction, documentation quality |

### Golden Triangle vs Single Agent

| When | Use |
|------|-----|
| Standard `:fast`, `:hard` variants | Single agent per phase |
| `:team` variant | Golden Triangle per phase |
| User explicitly requests team review/collaboration | `:team` variant |
| Maximum quality with adversarial debate is priority | `:team` variant |

### Golden Triangle Definitions Location

```
agents/teams/{team-name}/
├── techlead.md    # Coordinator, decomposer, arbitrator
├── executor.md    # Builder, implementer, defender
└── reviewer.md    # Devil's advocate, quality gatekeeper

# Wiki Team definitions:
agents/teams/wiki-team/
├── techlead.md    # Wiki Architect role — decomposes, coordinates, arbitrates
├── executor.md    # Wiki Extractor role — extracts, writes, defends
└── reviewer.md    # Wiki Reviewer role — validates accuracy, completeness, coverage
```

### Communication Protocol

- **Shared Task List**: Published by Tech Lead at phase start, tracks task status
- **Mailbox**: `./.reports/{topic}/MAILBOX-{date}.md` — append-only log of all exchanges
- **Debate**: Max 3 rounds per task → Tech Lead arbitrates
- **Consensus**: `✅ CONSENSUS: TechLead ✓ | Executor ✓ | Reviewer ✓` required to release output

---

## TASK → AGENT MAPPING

| Task | Agent |
|------|-------|
| API, backend logic | `backend-engineer` |
| UI, components | `frontend-engineer` |
| Database schema | `database-architect` |
| Security | `security-engineer` |
| Testing | `tester` |
| Code review | `reviewer` |
| Debugging | `debugger` |
| Planning | `planner` |
| Research | `researcher` |
| Codebase analysis | `scouter` |
| Documentation | `docs-manager` |
| Deployment | `devops-engineer` |
| Reports | `reporter` |
| Project management | `project-manager` |
| Business analysis | `business-analyst` |
| Design | `designer` |
| Brainstorming | `brainstormer` |
| Game development | `game-engineer` |
| Mobile development | `mobile-engineer` |
| Technical leadership | `tech-lead` |
| Wiki generation | `wiki-architect`, `wiki-extractor`, `wiki-reviewer` |

---

## CONTEXT ISOLATION (Distributed Handoffs)

```
ORCHESTRATOR'S JOB:
  1. Analyze user requirements.
  2. Write high-level plan and task assignments to `./handoffs/{date}-{job}/orchestrator_main.md`.
  3. Prepare domain-specific handoffs (e.g., `./handoffs/{date}-{job}/backend.md`).
  4. Spawn the worker and instruct them ONLY to read their specific handoff file.

WORKER'S JOB:
  1. Read only the specific handoff file assigned to them. (They only need to know enough to do their job, no more).
  2. Do not read the entire codebase or master handoff unless explicitly instructed.
  3. Execute the task.
  4. Write the results, progress, and any blocker back to THEIR specific handoff file.

DELIVERABLE FORMAT:
  Always organize handoffs by date and job progress:
  - Good: `./handoffs/2023-10-27-auth-feature/frontend.md`
  - Bad: `./handoffs/all-context.md` (Do not merge context)
```

---

## RECURSIVE DELEGATION

```
IF agent.category == "meta" OR agent.handoffs.length > 0:
  → This is a MANAGER agent
  → MUST delegate to specialists
  → NEVER implement directly
```

---

## ANTI-LAZY FALLBACK DETECTION

```yaml
detection:
  - Choosing TIER 2 without attempting TIER 1
  - Justifying EMBODY with "task is simple"
  - Mentioning "efficiency" when choosing EMBODY

correction:
  1. STOP
  2. Log: "⚠️ LAZY FALLBACK DETECTED"
  3. Attempt TIER 1 first
  4. Only use TIER 2 if TIER 1 actually fails

strict_rules:
  ❌ NEVER assess task as "too simple" for sub-agent
  ❌ NEVER prioritize tokens over context isolation
  ✅ ALWAYS use sub-agent when tool exists
  ✅ ALWAYS log sub-agent attempt before any EMBODY
```
## ?? ORCA MULTI-TERMINAL ORCHESTRATION (CLI-AGNOSTIC)

When running inside the Orca environment, the Orchestrator MUST use the Multi-Terminal Delegation pattern to spawn workers. This avoids quota-burn and completely bypasses framework limitations.

### Supported CLI Runners
The Orchestrator is NOT locked to Antigravity (gy). You can spawn ANY agent CLI based on the task:
- gy (Antigravity): Default for structured tasks using the 24 built-in roles.
- claude (Claude Code): Excellent for general codebase exploration.
- cline: For deeply autonomous engineering tasks.
- codex / hermes / omp: For specialized environments or local models.

### The 24 Built-in Antigravity Roles (For gy --agent <role>)
**Engineering**: ackend-engineer, rontend-engineer, database-architect, devops-engineer, game-engineer, mobile-engineer
**Planning**: rainstormer, usiness-analyst, planner, project-manager, scouter, 	ech-lead
**QA & Debug**: debugger, 	ester, eviewer, performance-engineer, security-engineer
**Docs & Research**: docs-manager, eporter, esearcher, designer, wiki-architect, wiki-extractor, wiki-reviewer

### The 5-Step Orchestration Lifecycle
To prevent infinite loops ("Inception") and resource leaks (infinite terminals), every delegation MUST strictly follow this lifecycle:

0. **CHECK MODELS (Pre-Check)**:
   Run a command (e.g. `agy --list-models` or `agy models`) to verify which models are currently available and active. Use this information to assign the most appropriate model to the worker.

1. **CREATE (Spawn)**:
   Use orca terminal create to spawn the worker. 
   *CRITICAL*: ALWAYS use the -p (print/one-shot) flag so the process exits when done, and ALWAYS prepend [ROLE: WORKER] to trigger the role switch.
   *Example*: orca terminal create --command "agy --agent devops-engineer -p '[ROLE: WORKER] Check docker-compose.yml'"
   *(Note the returned terminal ID, e.g., term_123)*

2. **WAIT (Sleep)**:
   Wait for the worker to finish the task.
   *Example*: orca terminal wait --terminal term_123 --for tui-idle

3. **READ (Extract)**:
   Scrape the output of the terminal to get the worker's answer.
   *Example*: orca terminal read --terminal term_123

4. **CLOSE (Cleanup)**:
   Kill the terminal pane to prevent infinite visual clutter.
   *Example*: orca terminal close --terminal term_123

