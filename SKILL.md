---
name: agent-craft
description: An active orchestration skill that designs and scaffolds new, deterministic AI Agent skills based on strict framework rules.
---
# Agent Crafter: Master Orchestrator

## 1. Discovery Phase Blueprint
**Goal:** Analyze the user's request for a new agent and design the architecture.
**Required Action:** You MUST read `agent-skill-framework.md` and `SKILL_EVOLUTION_RULES.md` before proposing any architecture. 
**Output:** A detailed blueprint proposing the required inputs, outputs, and compute tools for all needed sub-skills.
**Verification Gate (🛑):** Do not proceed to the Delivery Phase until the user has explicitly approved the blueprint.

## 2. Delivery Phase: Foundation Setup
**Goal:** Initialize the new agent repository.
**Input:** The approved Blueprint from the Discovery Phase AND the directory architecture template defined in `agent-skill-framework.md`.
**Output:** The initialized repository directory matching the framework's template structure.
**Tools Needed:** Standard file creation/writing tools (no custom compute tools needed).
**Action:** Create the required directory structure and foundational files exactly matching the architecture defined in `agent-skill-framework.md`.
**Verification Gate (🛑):** Verify that the created folder structure is correct and completely aligns with the template defined in `agent-skill-framework.md`.

## 3. Delivery Phase: Sub-Skill Generation
**Goal:** Build the individual composable sub-skills and Layer 3 compute tools (`tools/*.py`).
**Input:** The approved Blueprint detailing the specific inputs, outputs, and logic for each required sub-skill.
**Output:** Completed `skills/*/SKILL.md` playbooks and `skills/*/tools/*.py` compute tools.
**Tools Needed:** Code generation logic and file writing tools.
**Action:** Iteratively generate each sub-skill playbook and its associated deterministic tools as defined in the blueprint.
**Verification Gate (🛑):** Check that each required sub-skill folder exists, contains a `SKILL.md` playbook, and all associated Python compute tools in the `tools/` folder are created and syntactically valid.
