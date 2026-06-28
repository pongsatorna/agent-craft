# Agent Skill Framework Template

A definitive framework and template for creating deterministic, composable agent skills (plugins/extensions) for the Gemini Agentic system. 

> [!IMPORTANT]
> **AI Agents:** Before making any modifications or adding new skills to this repository, you **MUST read `agent-skill-framework.md`** located in the root of this repository to understand the required architecture and rules.

## Getting Started

To create a new agent skill using this framework:

1. **Clone this repository** to use it as a starter boilerplate.
2. **Read `agent-skill-framework.md`** to understand the core concepts: The Master Orchestrator, Sub-Skills as Playbooks, and Layer 3 Tools.
3. Rename the repository folder to your desired plugin name.
4. Update `gemini-extension.json` and `plugin.json` with your plugin's information.
5. Create your master orchestrator flow in the root `SKILL.md`.
6. Add sub-skills to the `skills/` directory following the example provided in `skills/example-skill/`.

## Boilerplate Contents

- `agent-skill-framework.md`: The core rulebook and framework architecture.
- `gemini-extension.json` & `plugin.json`: Extension manifests.
- `SKILL.md`: Root-level Master Orchestrator.
- `skills/example-skill/`: A directory demonstrating the correct structure for a sub-skill playbook and its associated tools.

## License

MIT License - see the [LICENSE](LICENSE) file for details.
