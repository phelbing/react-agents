# react-agents

4 subagents with model routing (haiku, sonnet) for frontends built with React, TypeScript and Vite: implementation, tests, review and test runs. Plus the React conventions and a frontend security checklist. Builds on `developer-workflow-agents` (separate repo).

## Installation

`react-agents` builds on `developer-workflow-agents`. The marketplaces of the dependencies must be added first, otherwise the plugin does not load:

```bash
claude plugin marketplace add phelbing/developer-workflow-agents
claude plugin marketplace add phelbing/react-agents
claude plugin install react-agents@react-agents
```

The dependency `developer-workflow-agents` is installed automatically. Then run `/agents` in Claude Code. The agents carry the plugin prefix, e.g. `react-agents:react-code-reviewer`.

## Contents

| Model | Agents |
|---|---|
| sonnet | react-implementer, react-test-writer, react-code-reviewer |
| haiku | frontend-test-runner |

Skills:
- `react-conventions`: TypeScript, components and hooks, state and data, accessibility, Vite, tests, tooling.
- `frontend-security-checklist`: XSS, URL handling, exposed environment variables, token storage, postMessage, third-party code, source maps, dependencies.
- `react-delegation-routing`: routing table and mandatory use.

The `react-code-reviewer` covers the security checklist as well. A separate security agent is not included.

## Structure of the conventions

Every rule exists exactly once, on the most general level it applies to. Higher levels load the lower ones and only add to them.

| Level | Plugin | Skill |
|---|---|---|
| Base (all stacks) | developer-workflow-agents | `engineering-conventions` |
| React | react-agents | `react-conventions`, `frontend-security-checklist` |

The agents load their skills at startup through the `skills` field in the frontmatter, including all levels below. In the main conversation, a skill loads the level below through the Skill tool. If a skill cannot be loaded, the agents say so in their report.

## Recommended additions to your project

A plugin does not load a `CLAUDE.md` or permissions into the project. Both are provided as templates under `examples/`:

- `examples/CLAUDE.template.md`: block for your own `CLAUDE.md`. Fill in the package manager and the commands.
- `examples/settings.json`: permissions for `.claude/settings.json`. Allows `npm run`, `npm audit`, the type check, single test runs and read-only `git` commands, blocks `npm publish`, `ssh`/`scp`/`rsync` and reading `.env*`. `npm run` allows every script in `package.json`. Adjust the entries for pnpm, yarn or bun.

## Notes

- Do not set `CLAUDE_CODE_SUBAGENT_MODEL`, otherwise the variable overrides the `model:` lines of all agents.
- To change an agent's model: the `model:` line in the frontmatter under `agents/`.
- Read third-party agents before using them and check their tool permissions.
- The review agent has `Bash` for `git diff`. "Read-only" is an instruction there, not a technical block. If you want it enforced, remove `Bash` and pass the diff in the prompt.
- The `deny` rules in `examples/settings.json` match the command as written. `rm -fr` or `/bin/rm -rf` are not covered. The rules guard against mistakes, they are not a hard block.

## For maintainers

```bash
claude plugin validate ./
```

`version` in `.claude-plugin/plugin.json` is set. Users stay on this version until you raise it. Dependencies without a version range follow the current state of the other plugins. For fixed version ranges, tag the releases with `claude plugin tag --push` and add the ranges to `dependencies`.

## License

MIT, see `LICENSE`.
