# My Orchestrator Skill

A specialized Orchestration Skill for AI Coding Assistants (Antigravity, Claude Code, Cline) running inside the **Orca** terminal multiplexer environment.

## Problem Solved

When using robust meta-agent frameworks (like `agent-assistant`), global rules strictly command the agent: *"YOU ARE THE ORCHESTRATOR. DO NOT IMPLEMENT. ALWAYS DELEGATE."*
However, when the Orchestrator delegates to a subagent, that subagent inherits the same global rules. This causes an **infinite loop (Inception)** where the subagent also believes it is an Orchestrator and refuses to code.

Additionally, native framework subagent calls (like `invoke_subagent`) often inherit the parent's expensive model tier (e.g., Opus 4.6), burning quota rapidly on simple tasks like file reading.

## Solution

This skill introduces **TIER 0: Orca Multi-Terminal Orchestration**.

1. **Quota Preservation**: Instead of native `invoke_subagent`, the Orchestrator uses `orca terminal create` to spawn CLI-agnostic agents (like `agy -m gemini-3.8-flash-high`) in separate terminal splits.
2. **Inception Fix (Role Switch)**: Global rules are updated with a `CRITICAL EXCEPTION FOR SUBAGENTS`. When the Orchestrator spawns a worker, it prepends `[ROLE: WORKER]` to the prompt. The worker reads this, disables its Orchestrator rule, and executes the task directly.
3. **Resource Cleanup**: The Orchestrator follows a strict 4-step lifecycle (CREATE, WAIT, READ, CLOSE) to prevent terminal leaks.

## Repository Structure

- `SKILL.md`: The Antigravity skill declaration.
- `rules/CORE.md`: The core rule file containing the `CRITICAL EXCEPTION FOR SUBAGENTS`.
- `rules/AGENTS.md`: The updated tier list and multi-terminal execution protocols.
- `rules/GEMINI.md`: The global framework configuration template.

## Usage

See `SKILL.md` for installation instructions and workflow details.
