---
name: agent-craft
description: An active orchestration skill that designs and scaffolds new, deterministic AI Agent skills based on strict framework rules.
---
# Agent Crafter: Master Orchestrator

This repo is meant to be cloned **once** and reused for every new agent you build. Each run of this orchestrator scaffolds a new plugin into `output/<plugin-name>/`, leaving the template itself untouched — see Phase 3.

## 1. Requirements Interview

**Goal:** Extract a complete, unambiguous picture of the requested agent directly from the user before proposing any architecture.
**Action:** Ask the following anchor questions. All of them are required — do not skip one because the user's initial request seems to already answer it; confirm explicitly instead.

1. **Identity** — What should this agent be called, and in one sentence, what does it do?
2. **Scope** — What real-world task or document type will it process? (e.g. "condo purchase quotations," "SQL migration scripts")
3. **Inputs** — What raw material does it start from, and in what format/location? (files, a folder, an API, chat messages)
4. **Outputs** — What should the finished result look like, and where should it be written?
5. **Existing tools/integrations** — Does this task depend on any specific tool, API, script, or data source the user already has? Do not assume one exists because it would make sense — ask.
6. **Sub-tasks** — What are the distinct steps a human currently does by hand, in order? (These become candidate sub-skills.)
7. **Rules to enforce** — Are there existing bylaws, policies, or criteria the agent must check against? Ask for the actual source document — never invent rule content.
8. **Persona/tone** — Should the agent have a name/persona and a specific tone (advisory vs. authoritative, formal vs. casual)?
9. **Boundaries** — Are there files, folders, or actions the agent must never touch or modify?
10. **Human checkpoints** — Where, if anywhere, should the agent pause for explicit approval before continuing?

**Anti-Hallucination Rules (MUST follow):**
- Never state that a tool, API, integration, or file format exists unless the user confirmed it. If you infer one from context, ask directly ("Do you already have X, or does this need to be built?") rather than asserting it.
- If an answer is missing or ambiguous, ask one bounded follow-up (max 2 per anchor question) before moving on. Do not fill the gap with a plausible-sounding default.
- The user may say "use your best judgment" for a given question — that explicitly authorizes you to propose a default, but it must be labeled **Proposed**, not **Confirmed**, in the blueprint (see Phase 2).
- Do not expand scope beyond what was discussed. If you think of a sub-skill the user didn't mention, offer it as a labeled suggestion in the blueprint — never fold it in silently as if it were requested.

**Output:** A structured record of answers to all 10 anchor questions, each tagged **Confirmed** (the user stated it) or **Proposed** (you're suggesting it, pending approval).
**Verification Gate (🛑):** Do not proceed to Phase 2 until every anchor question has a Confirmed or explicitly-authorized Proposed answer. No blanks.

## 2. Blueprint Synthesis & Approval

**Goal:** Turn the interview record into a concrete architecture.
**Required Action:** You MUST read `agent-skill-framework.md` and `SKILL_EVOLUTION_RULES.md` before proposing any architecture.
**Action:** For each sub-task identified in the interview, define it as a candidate sub-skill with explicit **Input**, **Output**, and **Tools Needed** (per `agent-skill-framework.md` Section 2). Present the full blueprint back to the user, with every detail still marked **Confirmed** or **Proposed** exactly as tagged in Phase 1 — this lets the user spot and correct anything you inferred, at a glance.
**Output:** The approved blueprint, including the plugin's `name` and one-line description (needed for Phase 3's manifests).
**Verification Gate (🛑):** Do not proceed to Phase 3 until the user has explicitly approved the blueprint. If they change anything, re-confirm the updated blueprint before proceeding — don't assume a partial edit approves the rest.

## 3. Foundation Setup

**Goal:** Initialize the new agent's directory **inside this repo**, without touching the template itself.
**Input:** The approved Blueprint AND the directory architecture template defined in `agent-skill-framework.md`.
**Output:** A new directory at `output/<plugin-name>/` (using the plugin `name` from the approved blueprint) containing:
- `gemini-extension.json` and `plugin.json`, populated with the new plugin's own `name` — never left as the `agent-craft`/`pongsatorna` template placeholders.
- `AGENTS.md`, generated from this repo's `AGENTS.md.example`, with persona, orchestration-flow summary, tone rules, and project boundaries filled in from the approved blueprint.
- A copy of `agent-skill-framework.md` and `SKILL_EVOLUTION_RULES.md`, so the new agent can keep following the framework and self-evolve once its `output/<plugin-name>/` folder is extracted into its own repository.
**Tools Needed:** Standard file creation/writing tools (no custom compute tools needed).
**Action:** Create `output/<plugin-name>/` and populate it with the structure above, matching `agent-skill-framework.md` exactly. Never write plugin files outside `output/`, and never overwrite this template's own root-level `SKILL.md`, `agent-skill-framework.md`, or `SKILL_EVOLUTION_RULES.md`.
**Verification Gate (🛑):**
- `output/<plugin-name>/` exists and its folder structure completely aligns with the template.
- `gemini-extension.json`/`plugin.json` inside it contain the new plugin's actual name — neither still says `"agent-craft"` / `"pongsatorna"`.
- `AGENTS.md`, `agent-skill-framework.md`, and `SKILL_EVOLUTION_RULES.md` are all present inside `output/<plugin-name>/`.
- Nothing outside `output/<plugin-name>/` was modified.

## 4. Sub-Skill Generation

**Goal:** Build the individual composable sub-skills and Layer 3 compute tools inside `output/<plugin-name>/skills/`.
**Input:** The approved Blueprint detailing the specific inputs, outputs, and logic for each required sub-skill.
**Output:** Completed `output/<plugin-name>/skills/*/SKILL.md` playbooks and `output/<plugin-name>/skills/*/tools/*.py` compute tools.
**Tools Needed:** Code generation logic and file writing tools.
**Action:** Iteratively generate each sub-skill playbook and its associated deterministic tools as defined in the blueprint. The moment a generated tool imports a third-party package, add it to `output/<plugin-name>/requirements.txt` immediately, replacing the placeholder comment lines on first real use — do not defer this to a later cleanup pass.
**Verification Gate (🛑):** Check that each required sub-skill folder exists under `output/<plugin-name>/skills/`, contains a `SKILL.md` playbook, and all associated Python compute tools in its `tools/` folder are created and syntactically valid. Confirm every third-party import across all generated tools has a matching entry in `output/<plugin-name>/requirements.txt`.

## 5. Final Compliance Gate

**Goal:** Confirm the fully assembled plugin in `output/<plugin-name>/` is free of the framework's known anti-patterns before handing it back to the user.
**Input:** The complete generated directory from Phases 3–4.
**Action:** Explicitly check `output/<plugin-name>/` against every anti-pattern listed in `agent-skill-framework.md` Section 6, one by one:
- **Buried Orchestrator** — `SKILL.md` sits at `output/<plugin-name>/`'s root, not nested inside `skills/`.
- **Hardcoded Filenames** — generated tools read directories/glob patterns, not literal filenames.
- **Scripts Directory** — no `scripts/` folder exists anywhere inside it; all executable code lives under `skills/*/tools/`.
- **Missing `gemini-extension.json`** — the file exists and its `name` field matches the new plugin.
- **Orphaned Manifests** — `plugin.json`'s `name` field also matches the new plugin.
**Output:** A short compliance summary shown to the user, plus a reminder that `output/<plugin-name>/` can now be extracted into its own repository (`cd output/<plugin-name> && git init`) whenever they're ready to ship it — this template repo stays clean and reusable for the next agent.
**Verification Gate (🛑):** Do not consider the plugin complete until all five checks pass. If any fails, fix the specific file inside `output/<plugin-name>/` and re-run this phase.
