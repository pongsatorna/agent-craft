# agents.md

## Agent Persona
The designated agent for this project is:
- **Name:** <TODO: agent's display name — leave as TODO if the user hasn't specified one>
- **Role:** <TODO: one-line description of what this agent does, e.g. "Committee Co-Pilot">

She/He/It must refer to itself by this name and sign off all reports as **<Name>** (<Role>).

## Project Overview
<TODO: 2-3 sentences on what this skill does and who/what it evaluates or produces>

## Setup & Environment
Run standard package setup:
```bash
pip install -r requirements.txt
```

## Multi-Agent Orchestration Flow
<TODO: describe each phase from this repo's root SKILL.md in plain language — invocation command, output, and verification gate for each. Keep this in sync with SKILL.md; if a phase changes there, update it here too (per SKILL_EVOLUTION_RULES.md Rule 2, this is an L4 update).>

1. **Phase 1: <name>**
   - **Invocation**: <command or SKILL.md phase reference>
   - **Output**: <file(s) produced>

2. **Phase 2: <name>**
   - **Invocation**: <command or SKILL.md phase reference>
   - **Output**: <file(s) produced>

<...continue for each phase in SKILL.md...>

## Agent Style & Tone Rules
- <TODO: tone/voice rules specific to this agent, e.g. "phrase findings as recommendations, not rulings">
- **Self-Evolution Rule**: The executing agent must follow the guidelines in [SKILL_EVOLUTION_RULES.md](./SKILL_EVOLUTION_RULES.md) to detect gaps during execution and propose cascaded updates (L1-L4) to the playbooks or tools when improvements are needed.
- **Architecture Rule**: When modifying the codebase, creating new tools, or restructuring files, the executing agent must read and adhere to the design guidelines in [agent-skill-framework.md](./agent-skill-framework.md).

## Project Boundaries
The executing agent must not modify or track files in:
- <TODO: this skill's own temp/output directories, e.g. `inbox/<folder_name>/temp/`, `inbox/<folder_name>/output/`>
- `.agents/`
