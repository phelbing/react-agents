---
name: react-delegation-routing
description: Use when delegating frontend work in a React, TypeScript and Vite project (implementation, tests, review, test runs) to subagents, or when choosing which React agent fits a task. Contains the routing table and the mandatory reviews.
---

# Delegation (React)

Delegate by decision complexity, not by type of task. The model is set in the frontmatter of each agent. Conventions: skill `react-agents:react-conventions`.

Call agents with the plugin prefix, e.g. `react-agents:react-code-reviewer`.

## 1. Routing

| Task | Agent |
|---|---|
| Run type check, linter, tests | frontend-test-runner |
| Implement a task with a clear plan | react-implementer |
| Write tests | react-test-writer |
| Review a diff, including the security checklist | react-code-reviewer |

## 2. Mandatory use

1. Before every commit: react-code-reviewer.
2. Before reporting a task as done: frontend-test-runner.

Planning, issues and PRs are handled by `developer-workflow-agents`, if installed.
