---
name: my-agent-plugin
description: The master orchestrator for this plugin. It routes to appropriate sub-skills based on conditions.
---

# Master Orchestrator

This is the root `SKILL.md` acting as the Master Orchestrator. It dictates exactly *when* to trigger sub-skills and defines strict "Gates" to prevent hallucination.

## Discovery Phase Blueprint
*(This section is generated during planning to outline inputs, outputs, and tools before execution begins).*

## Delivery Phase 1: Setup and Fundamentals
**Goal:** Initialize the environment and required folder structures.
**Verification Gate (🛑):** Check that the directories exist before proceeding.

## Delivery Phase 2: Example Composable Skill
**Input:** Raw user request string.
**Output:** Processed JSON output.
**Tools Needed:** `example.py` (Existing compute tool to be executed).
**Action:** Invoke `example-skill` to execute the playbook.
**Verification Gate (🛑):** Check that the JSON output indicates success.
