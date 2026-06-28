---
name: skill-evolution-rules
description: General rules for agents to identify, propose, and apply essential updates to cloned skills after real-world usage.
version: 1.0.0
---

# Skill Evolution Rules

When an agent clones and uses a skill (playbook + tools), it MUST follow these rules to keep the skill accurate, robust, and self-improving.

---

## Rule 1: Detect Gaps During Execution

After each phase execution, the agent MUST evaluate:

| Check | Question |
|-------|----------|
| **Input Mismatch** | Did the actual input format differ from what the skill documented? |
| **Tool Failure** | Did any tool fail due to unhandled data, missing logic, or wrong assumptions? |
| **Missing Handling** | Was there a real-world pattern (e.g., DML type, column name, edge case) the tool didn't cover? |
| **Dependency Issue** | Did a pinned version fail to install? Was a new dependency needed? |
| **Cycle/Error Resolution** | Did the agent need manual intervention to resolve an error the tool should have handled? |

If ANY answer is "yes", the agent MUST propose an update.

---

## Rule 2: Classify the Update Level

Every proposed change must be classified by scope:

| Level | Scope | What to Update | Example |
|-------|-------|----------------|---------|
| **L1 - Tool** | Python script logic | `tools/*.py` | Add support for new SQL statement type |
| **L2 - Sub-Skill** | Skill playbook instructions | `skills/*/SKILL.md` | Document new input format, add validation step |
| **L3 - Orchestrator** | Phase flow or gating | `SKILL.md` (root) | Add new phase, change gate conditions |
| **L4 - Project** | Entry points, dependencies, docs | `README.md`, `requirements.txt`, `run_pipeline.py` | Add new tool, update usage instructions |

**Rule: Updates MUST cascade upward.** If L1 changes tool behavior, L2 must reflect the new capability, L3 must know about it, and L4 must document it for users.

---

## Rule 3: Propose Before Applying

The agent MUST structure proposals as:

```
## Proposed Skill Update

**Trigger:** [What happened during execution that revealed the gap]
**Level:** L1/L2/L3/L4
**Files affected:**
- `path/to/file` — [what changes]

**Rationale:** [Why this makes the skill smarter for next use]

**Breaking change:** Yes/No
```

If the user is present, wait for approval. If running autonomously, apply non-breaking changes (L1-L2) and flag breaking changes (L3-L4) for review.
