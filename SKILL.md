---
name: my_orchestrator_skill
description: "Orca Multi-Terminal Orchestration Skill for Antigravity & Agent Assistants. Overrides default framework constraints to allow tier-0 terminal-based multi-agent coordination without infinite inception loops."
---

# Orca Multi-Terminal Orchestration Skill

This skill configures an Antigravity agent (or similar framework like Claude Code/Cline) to operate strictly as an **Orchestrator** (Tier 0). 

It prevents the "Inception" bug (agents spawning agents infinitely) by establishing a strict multi-terminal orchestration pattern where the Orchestrator uses `orca terminal create` to spawn workers with a `[ROLE: WORKER]` prefix, instructing them to drop the Orchestrator ruleset and execute directly.

## Installation & Setup

1. Copy the contents of `rules/CORE.md` and `rules/AGENTS.md` to your local `~/.gemini/antigravity/skills/agent-assistant/rules/` directory (or equivalent framework config).
2. Copy `rules/GEMINI.md` to your global `~/.gemini/GEMINI.md` file to enforce the Role Switch exception globally.

## The 4-Step Orchestration Lifecycle

When operating in Orca, the Orchestrator follows these 4 steps:
1. **CREATE**: `orca terminal create --command "agy --agent <role> -p '[ROLE: WORKER] <task>'"`
2. **WAIT**: `orca terminal wait --terminal <id> --for tui-idle`
3. **READ**: `orca terminal read --terminal <id>`
4. **CLOSE**: `orca terminal close --terminal <id>`

## Built-in Roles (agy agents)

The orchestration pattern is CLI-agnostic, but if using `agy`, the 24 built-in roles categorized in `rules/AGENTS.md` are natively supported (e.g., `devops-engineer`, `backend-engineer`, `tester`).
