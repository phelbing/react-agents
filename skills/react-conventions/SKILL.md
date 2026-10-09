---
name: react-conventions
description: Use when writing, changing, testing or reviewing code in a React, TypeScript and Vite frontend. Conventions for TypeScript, components, hooks, state, data loading, accessibility, Vite configuration, tests and tooling, built on the shared engineering conventions.
---

# React conventions

First load `developer-workflow-agents:engineering-conventions` through the Skill tool, if not done yet. The rules there still apply. This skill only adds what is specific to React, TypeScript and Vite.

The versions of React, TypeScript and Vite are in `package.json`. Do not use APIs above them. Look up API details in the documentation or under `node_modules/`, do not write them from memory. The project's configuration (`tsconfig.json`, ESLint, Prettier) and neighboring code take precedence.

## TypeScript

- Keep the `strict` settings of `tsconfig.json`. Do not loosen them to make an error go away.
- No `any`. Use `unknown` for data from outside (API responses, `localStorage`, `postMessage`) and narrow it with a type guard or a schema validation the project already uses.
- No type assertions (`as`) to silence the compiler. No non-null assertion (`!`) without a reason.
- Type props explicitly. Derive types from a single source instead of duplicating them.
- `import type` for type-only imports, if the project's configuration requires it.

## Components and hooks

- Function components only. One exported component per file, named like the file.
- Components render, hooks hold logic. Move logic that is reused or tested on its own into a custom hook (`use…`).
- Follow the rules of hooks: call hooks only at the top level of components and hooks, never in conditions or loops.
- Effects only to synchronize with something outside React (subscriptions, timers, browser APIs). No effect for values that can be computed during render, and no effect to react to a user event: handle it in the event handler.
- Every effect with a subscription, timer or request cleans up after itself.
- Stable, unique `key` values from the data. No array index as key for lists that change.
- No direct DOM manipulation. `ref` only where React has no API for it (focus, measuring, third-party widgets).

## State and data

- State as local as possible. Lift it up only as far as needed. Context for values many components need and that change rarely, not as a general store.
- Do not store derived values in state, compute them.
- Server data through the data loading library the project uses. No custom cache next to it. Without such a library: loading, error and empty state are handled explicitly, and outdated responses are discarded.
- API calls in one place per resource (client module or hook), not scattered across components.

## Accessibility

- Semantic HTML first: `button` for actions, `a` for navigation, headings in order, lists as lists.
- Every form field has a label. Every image has an `alt` text, decorative images `alt=""`.
- Everything works with the keyboard. Visible focus, no `div` with `onClick` as a button replacement.
- ARIA only where HTML has no element for it.

## Vite

- Configuration in `vite.config.ts`. Path aliases there and in `tsconfig.json` stay in sync.
- Environment variables through `import.meta.env`. Variables with the prefix from `envPrefix` (default `VITE_`) end up in the client bundle and are public. Security details: skill `react-agents:frontend-security-checklist`.
- Type custom environment variables in `ImportMetaEnv` (`vite-env.d.ts`).

## Tests

- Test tool from `package.json`, usually Vitest with Testing Library.
- Test behavior from the user's point of view: query by role, label or text, not by CSS class or test id when a role exists. Interactions with `userEvent`.
- One behavior per test, meaningful test names, Arrange-Act-Assert.
- Mock only at the boundaries (network, time, browser APIs). Use the network mock the project already uses.
- No snapshot tests as the only check of a component.
- Cover edge cases: loading, error and empty state, invalid input.
- If you find a bug in production code while testing, report it and do not fix it unasked.

## Tooling

- Commands from the `scripts` in `package.json`. Package manager from the lock file, do not mix package managers.
- Type check with the project's script, without one `tsc --noEmit`. Lint with the project's ESLint configuration.
- Run tests once, not in watch mode.
- Add dependencies only with a reason. Check first whether the platform, React or an existing dependency already covers it.
