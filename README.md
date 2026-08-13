# Agent Skill Framework Template

A definitive framework for creating deterministic, composable agent skills for the Gemini Agentic system.

**🔥 THIS IS AN ACTIVE AGENT SKILL 🔥** This repository is not just a passive template—it is an active "Agent Crafter" plugin. When you install this plugin, the agent gains the ability to autonomously design and scaffold new, deterministic AI Agent skills based on the strict rules defined in this framework.

> **AI Agents:** Before making any modifications or adding new skills to this repository, you **MUST read two files** located in the root of this repository:
> 1. `agent-skill-framework.md` (for the required architecture and structure)
> 2. `SKILL_EVOLUTION_RULES.md` (for rules on how to self-improve and propose updates)

## Getting Started (one-time)

1. **Clone this repository once.** You do not need a fresh clone per agent — see "Building Multiple Agents" below.
2. **Read `agent-skill-framework.md`** to understand the core concepts: The Master Orchestrator, Sub-Skills as Playbooks, and Layer 3 Tools.
3. **Read `SKILL_EVOLUTION_RULES.md`** to understand how to maintain and evolve skills over time.

## Building an Agent

Invoke the root `SKILL.md` (the "Agent Crafter") and describe what you want to build. It will:

1. **Interview you** with a fixed set of anchor questions (identity, scope, inputs/outputs, existing tools, sub-tasks, rules to enforce, persona, boundaries, approval points) — see `SKILL.md` Phase 1. It will not invent tools, integrations, or rules you haven't confirmed; anything it proposes is explicitly labeled so you can approve or reject it.
2. **Synthesize a blueprint** from your answers and ask for your explicit approval before writing anything.
3. **Scaffold the new plugin** into `output/<plugin-name>/` — not into this repo's root, and not by renaming this repo. The template stays clean.
4. **Generate the sub-skills and tools** for the new plugin, keeping `output/<plugin-name>/requirements.txt` in sync as it goes.
5. **Run a final compliance check** against the anti-patterns in `agent-skill-framework.md` before handing the finished plugin back to you.

## Building Multiple Agents

Because every run writes to `output/<plugin-name>/` and never touches this repo's own root files, you can run the Agent Crafter as many times as you like from the same clone — one output subfolder per agent. `output/` is git-ignored by default, so your generated agents don't clutter this template's own history.

When an agent in `output/<plugin-name>/` is ready to ship, extract it into its own repository:
```bash
cd output/<plugin-name>
git init && git add . && git commit -m "Initial scaffold"
# then push to a new remote of your choice
```
From that point on, the extracted agent is self-contained — it carries its own copies of `agent-skill-framework.md`, `SKILL_EVOLUTION_RULES.md`, and `AGENTS.md`, so it can keep following the framework and propose its own updates without depending on this template repo still being reachable.

## Boilerplate Contents

- `agent-skill-framework.md`: The core rulebook and framework architecture.
- `gemini-extension.json` & `plugin.json`: Extension manifests for this Agent Crafter skill itself.
- `SKILL.md`: Root-level Master Orchestrator — a 5-phase flow (Requirements Interview, Blueprint Synthesis & Approval, Foundation Setup, Sub-Skill Generation, Final Compliance Gate).
- `AGENTS.md.example`: Fill-in-the-blanks persona/orchestration/tone/boundaries scaffold, copied into every `output/<plugin-name>/AGENTS.md`.
- `SKILL_EVOLUTION_RULES.md`: Self-improvement rules, copied into every generated plugin.
- `skills/example-skill/`: A directory demonstrating the correct structure for a sub-skill playbook and its associated tools.
- `output/`: Where every generated plugin lands, one subfolder per plugin name. Git-ignored.

## License

MIT License - see the [LICENSE](./LICENSE) file for details.
