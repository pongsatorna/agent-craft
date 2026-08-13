# Gemini Agent Skill Framework (Best Practices & Architecture)

This document serves as the definitive framework for creating deterministic, composable agent skills (plugins/extensions) for the Gemini Agentic system. It is based on lessons learned from building the Integration Architect and Batch Dependency Analyzer agents.

## 1. Directory Architecture
A robust agent plugin must adhere to the following file structure to be correctly parsed and installed via the `agy cli`. When this framework's own Agent Crafter (`SKILL.md`) scaffolds a new plugin, this structure is created under `output/<plugin-name>/`, not at the Agent Crafter's own repo root — see that repo's `SKILL.md` Phase 3.

```text
my-agent-plugin/
├── gemini-extension.json     # Required: Declares the repo as an extension package
├── plugin.json               # Required: Minimal configuration (usually just {"name": "plugin-name"})
├── SKILL.md                  # Required: The "Master Orchestrator" entry point
├── AGENTS.md                 # Required: Agent persona, orchestration-flow summary, tone rules, and project boundaries
├── README.md                 # Required: User documentation (how to install/use)
├── agent-skill-framework.md  # Required: A copy of this rulebook, so the scaffolded agent can self-govern once detached
├── SKILL_EVOLUTION_RULES.md  # Required: A copy of the self-evolution rules, for the same reason
├── requirements.txt          # Optional: Python dependencies for Layer 3 tools
└── skills/                   # Required: Directory for all composable sub-skills
    ├── sub-skill-a/
    │   ├── SKILL.md          # The execution playbook for Sub-Skill A
    │   └── tools/            # Python scripts, binaries, or utilities
    │       └── script_a.py
    └── sub-skill-b/
        ├── SKILL.md          # The execution playbook for Sub-Skill B
        └── tools/
            └── script_b.py
```

## 2. Planning Methodology (Anthropic Framework)
Before writing any code or executing tasks, the agent MUST propose a plan broken down into strict phases. This prevents hallucination and ensures deterministic execution.

- **1. Discovery Phase:** The agent analyzes the requirements and proposes all needed sub-skills. For each sub-skill, it MUST clearly define:
  - **Input:** What data/format goes into the phase.
  - **Output:** What data/format comes out of the phase.
  - **Tools Needed:** The specific compute tools required (specify if they exist or need to be developed).
  - *Outcome:* A complete blueprint for the next phase.
- **2. Delivery Phase:** The execution of the blueprint.
  - **First Step:** Setup fundamentals, folder structures, and environments.
  - **Later Steps:** Developing and executing each of the composable sub-skills according to the blueprint.
- **Core Principle:** The agent MUST focus on creating or using compute tools (deterministic logic) rather than relying on the LLM to process data, calculate, or hallucinate logic.

## 3. The Master Orchestrator (Root `SKILL.md`)
The root `SKILL.md` does **not** do the actual computing work. Instead, it acts as a "Master Orchestrator" or traffic controller. It dictates exactly *when* to trigger sub-skills and defines strict "Gates" to prevent hallucination.

**Key Components:**
- **Phase Definition**: Break the workflow into phases.
- **Conditions**: What must be true to start the phase? (e.g., "Files exist in `./inbox/`")
- **Action**: Explicitly name the sub-skill to invoke (e.g., "Invoke `sub-skill-a`").
- **Goal**: What is the expected output? (e.g., "`output.json`")
- **Verification Gates (🛑)**: Explicit rules the agent MUST check before proceeding. If the goal isn't met, the agent must halt and not proceed to the next phase.
- **Final Compliance Gate**: The last phase MUST explicitly check the finished plugin against every rule in Section 6 (Anti-Patterns) before handover — a rule that is never gated against is a rule that will eventually be broken silently.

## 4. Sub-Skills as "Execution Playbooks"
Sub-skills live in `skills/<name>/SKILL.md`. They must be structured as strict, deterministic **Playbooks** rather than vague instructions.

**Standard Playbook Sections:**
1. **Environmental Scan & Pre-flight Check**: Instruct the agent to verify that required input directories (like `./inbox/`) exist, output files from previous phases are present, and required packages (`pip install -r requirements.txt`) are installed.
2. **Deterministic Execution (Compute Tools)**: Provide the exact terminal commands the agent must run.
   - *Crucial*: Scripts must reside in the `tools/` directory (e.g., `python tools/my_script.py`).
   - Use dynamic inputs (like an `./inbox/` directory) rather than hardcoding specific filenames (`my_data.csv`).
3. **Semantic Validation**: Tell the agent how to evaluate the output of the compute tool. (e.g., "Check if the JSON contains `"status": "success"`).
4. **Final Handover**: Instruct the agent to summarize its work and explicitly hand control back to the Master Orchestrator for the next phase.

## 5. Layer 3 Tools (`tools/`)
The AI should not be relied upon to perform complex math, parse ASTs, or do heavy data lifting. This logic belongs in Layer 3 Tools.
- Tools should be deterministic code (e.g., Python).
- Tools should output structured, readable data (like JSON or Markdown) so the agent can easily parse the result in the "Validation" step.
- The moment a tool needs a third-party package, it must be added to root `requirements.txt` in the same step — dependency drift is an L1→L4 cascade failure per `SKILL_EVOLUTION_RULES.md`.

## 6. Anti-Patterns to Avoid
- ❌ **Buried Orchestrator**: Do not place the master orchestration `SKILL.md` inside a sub-directory. It must sit at the root.
- ❌ **Hardcoded Filenames**: Do not design tools to look for `data123.csv`. Instead, use patterns like "read all `.csv` files in the `./inbox/` folder".
- ❌ **Scripts Directory**: Do not place python files in a `scripts/` folder; use `tools/`.
- ❌ **Missing `gemini-extension.json`**: Without this, the system may not recognize the repository as a full extension suite.
- ❌ **Orphaned Manifests**: Do not leave `gemini-extension.json` or `plugin.json` with the boilerplate template's own `name` (e.g. `"agent-craft"`). Every scaffolded plugin must carry its own identity in both files.

## 7. Propagating Framework Docs to Child Skills
Every skill scaffolded from this template will eventually be renamed and detached into its own standalone repository (see this template's README, "Getting Started" step 4). Once detached, it can no longer read this template's copy of `agent-skill-framework.md` or `SKILL_EVOLUTION_RULES.md`.

Because of this, **Foundation Setup MUST copy both files into the new plugin's own root** (see Section 1's directory tree). This lets the scaffolded agent keep following the architecture rules and propose self-evolution updates (per `SKILL_EVOLUTION_RULES.md`) entirely on its own, without depending on this template repo still being reachable. An `AGENTS.md` (see Section 1) should link to these local copies, not to paths inside the original `agent-craft` template.
