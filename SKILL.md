---
name: my-agent-plugin
description: The master orchestrator for this plugin. It routes to appropriate sub-skills based on conditions.
---

# Master Orchestrator

This is the root `SKILL.md` acting as the Master Orchestrator. It dictates exactly *when* to trigger sub-skills and defines strict "Gates" to prevent hallucination.

## Phase 1: Example Initialization

**Condition:** User requests to run the example process.
**Action:** Invoke `example-skill` to execute the playbook.
**Goal:** Successfully run the example tool and generate an output.
**Verification Gate (🛑):** Check that the output from the tool indicates success before proceeding to the next phase.

## Phase 2: Next Steps

*(Define subsequent phases here as needed)*
