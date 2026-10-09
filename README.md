# react-agents

4 subagents with model routing (haiku, sonnet) for frontends built with React, TypeScript and Vite: implementation, tests, review and test runs. Plus the React conventions and a frontend security checklist. Builds on [`developer-workflow-agents`](https://github.com/phelbing/developer-workflow-agents) (separate repo).

## 1. Installation

`react-agents` builds on `developer-workflow-agents`. The marketplaces of the dependencies must be added first, otherwise the plugin does not load:

```bash
claude plugin marketplace add phelbing/developer-workflow-agents
claude plugin marketplace add phelbing/react-agents
claude plugin install react-agents@react-agents
```

The dependency `developer-workflow-agents` is installed automatically. Then run `/agents` in Claude Code. The agents carry the plugin prefix, e.g. `react-agents:react-code-reviewer`.

## 2. Contents

| Model | Agents |
|---|---|
| sonnet | react-implementer, react-test-writer, react-code-reviewer |
| haiku | frontend-test-runner |

Skills:
1. `react-conventions`: TypeScript, components and hooks, state and data, accessibility, Vite, tests, tooling.
2. `frontend-security-checklist`: XSS, URL handling, exposed environment variables, token storage, postMessage, third-party code, source maps, dependencies.
3. `react-delegation-routing`: routing table and mandatory use.

The `react-code-reviewer` covers the security checklist as well. A separate security agent is not included.

## 3. Recommended additions to your project

A plugin does not load a `CLAUDE.md` or permissions into the project. Both are provided as templates under `examples/`:

1. `examples/CLAUDE.template.md`: block for your own `CLAUDE.md`. Fill in the package manager and the commands.
2. `examples/settings.json`: permissions for `.claude/settings.json`. Allows `npm run`, `npm audit`, the type check, single test runs and read-only `git` commands, blocks `npm publish`, `ssh`/`scp`/`rsync` and reading `.env*`. `npm run` allows every script in `package.json`. Adjust the entries for pnpm, yarn or bun.

## 4. Notes

1. Do not set `CLAUDE_CODE_SUBAGENT_MODEL`, otherwise the variable overrides the `model:` lines of all agents.
2. Read third-party agents before using them and check their tool permissions.
3. The review agent has `Bash` for `git diff`. "Read-only" is an instruction there, not a technical block. If you want it enforced, remove `Bash` and pass the diff in the prompt.
4. The `deny` rules in `examples/settings.json` match the command as written. `rm -fr` or `/bin/rm -rf` are not covered. The rules guard against mistakes, they are not a hard block.

## 5. Documentation

1. [docs/architecture.md](docs/architecture.md): how the conventions are layered and how the agents load their skills.
2. [docs/maintenance.md](docs/maintenance.md): validation, agent models and versions, for maintainers.

## 6. Related plugins

1. [`developer-workflow-agents`](https://github.com/phelbing/developer-workflow-agents): Agents for planning, debugging, refactoring, issues, pull requests and release notes, plus the shared engineering conventions. Base for all other plugins.
2. [`php-agents`](https://github.com/phelbing/php-agents): PHP and Doctrine agents for code search, tests, review, migrations, security and performance. Builds on developer-workflow-agents.
3. [`symfony-agents`](https://github.com/phelbing/symfony-agents): Symfony agents for implementation, tests, review and security. Builds on php-agents.

## 7. License

MIT, see `LICENSE`.
