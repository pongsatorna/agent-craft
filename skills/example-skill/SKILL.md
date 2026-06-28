---
name: example-skill
description: An example sub-skill playbook demonstrating the deterministic execution flow.
---

# Example Skill Playbook

This playbook demonstrates how a sub-skill should be structured according to the Agent Skill Framework.

## 1. Environmental Scan & Pre-flight Check
Verify that any required directories or files exist. For example:
- Check if `./inbox/` exists (if applicable to your task).
- Ensure required python packages are installed via `pip install -r requirements.txt`.

## 2. Deterministic Execution
Run the appropriate deterministic tools in the `tools/` directory.

```bash
# Example terminal command
python tools/example.py --input "Hello World"
```

## 3. Semantic Validation
Evaluate the output of the tool. 
- Ensure the tool executed successfully.
- Check if the generated output meets the expected schema or criteria.

## 4. Final Handover
Summarize the work completed in this sub-skill and explicitly hand control back to the Master Orchestrator for the next phase.
