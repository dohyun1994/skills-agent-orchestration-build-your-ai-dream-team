# Agent team

We will use a four-person custom agent team in GitHub Copilot CLI, running inside a Codespace, to coordinate the build of Mona's Project Pulse dashboard.

- Orchestrator — Model: Claude Opus 5.5 (copilot). Responsibility: coordinates the overall workflow, breaks the work into phases, delegates to the specialist agents, manages dependencies, and verifies the final result. Definition: `.github/agents/orchestrator.agent.md`.
- Planner — Model: Claude Opus 5.5 (copilot). Responsibility: researches the repo and requirements, identifies risks and dependencies, and produces the implementation plan and file ownership for the dashboard. Definition: `.github/agents/planner.agent.md`.
- Designer — Model: Gemini 3.7 Flash (copilot). Responsibility: shapes the user experience, visual hierarchy, and UI polish for the Project Pulse dashboard, including accessibility and responsive layout decisions. Definition: `.github/agents/designer.agent.md`.
- Coder — Model: GPT-5.5 (copilot). Responsibility: implements the dashboard code, support files, and validation work in the assigned file scope, including the runnable preview configuration for the app. Definition: `.github/agents/coder.agent.md`.

This setup uses GitHub Copilot CLI in a Codespace to orchestrate the work: the Orchestrator delegates to the Planner, Designer, and Coder, while the team collaborates on the Project Pulse implementation and handoff.
