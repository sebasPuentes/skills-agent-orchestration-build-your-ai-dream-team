# Agent team for Mona's Project Pulse dashboard

We will use a four-agent custom team to plan, design, coordinate, and implement the Project Pulse dashboard in GitHub Copilot CLI from a Codespace.

| Agent | Target model | Responsibility | Agent definition |
| --- | --- | --- | --- |
| Planner | Claude Opus 4.7 (copilot) | Researches the repo, reads relevant files and docs, identifies dependencies and risks, and creates the implementation plan the team will execute. | `.github/agents/planner.agent.md` |
| Orchestrator | Claude Opus 4.7 (copilot) | Breaks the plan into phases, assigns work to specialist agents, coordinates parallel or sequential execution, and verifies that the integrated work matches the requested outcome. | `.github/agents/orchestrator.agent.md` |
| Coder | GPT-5.5 (copilot) | Implements the code and logic within the scope assigned by the orchestrator, including support files needed for a runnable app such as the dashboard preview. | `.github/agents/coder.agent.md` |
| Designer | Gemini 3.1 Pro (copilot) | Handles UI/UX, accessibility, information hierarchy, interaction flow, and visual design for the dashboard, with an emphasis on polished, responsive project views. | `.github/agents/designer.agent.md` |

All four agents live under the repository's custom agent folder: `.github/agents/`. The team is orchestrated through the GitHub Copilot CLI in a Codespace, with the Orchestrator delegating work to the Planner, Coder, and Designer as the project evolves.
