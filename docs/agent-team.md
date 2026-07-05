# Agent team

This document summarizes the custom agent team designed to build Mona's Project Pulse dashboard.

## Agent Team Summary

### 1. Orchestrator
- **Target Model**: Claude Opus 4.7 (copilot)
- **Responsibility**: Coordinates the other agents, breaking down complex user requests into discrete tasks, establishing phases, and delegating file scopes to specialists.
- **Definition Path**: `.github/agents/orchestrator.agent.md`

### 2. Planner
- **Target Model**: Claude Opus 4.7 (copilot)
- **Responsibility**: Researches the repository, documentation, dependencies, and edge cases to formulate step-by-step implementation plans and validation expectations. Does not write code.
- **Definition Path**: `.github/agents/planner.agent.md`

### 3. Coder
- **Target Model**: GPT-5.5 (copilot)
- **Responsibility**: Implements code-oriented tasks, writes clean and explicit logic, fixes bugs, and configures runner environments (like `.vscode/launch.json`) within assigned scopes.
- **Definition Path**: `.github/agents/coder.agent.md`

### 4. Designer
- **Target Model**: Gemini 3.1 Pro (copilot)
- **Responsibility**: Directs UI/UX, accessibility, information hierarchy, interaction flows, and visual design to deliver a responsive, highly polished dashboard.
- **Definition Path**: `.github/agents/designer.agent.md`

---

> **Note**: This orchestration work is conducted using the **GitHub Copilot CLI** within a **GitHub Codespace** to manage and coordinate our custom AI agent team.
