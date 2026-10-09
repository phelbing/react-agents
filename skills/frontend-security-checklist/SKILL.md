---
name: frontend-security-checklist
description: Use for security reviews of a React, TypeScript and Vite frontend. Checklist for XSS, URL handling, exposed environment variables, token storage, postMessage, third-party code, source maps and dependencies, with report format.
---

# Frontend security checklist

First load `developer-workflow-agents:engineering-conventions` through the Skill tool, if not done yet (never print secrets).

## Checks

- **XSS:** `dangerouslySetInnerHTML` only with content sanitized by a sanitizer the project uses. No `innerHTML`, `outerHTML` or `document.write` with user data.
- **URLs from user input:** in `href`, `src` and redirects, allow only expected schemes (`https:`, `http:`, `mailto:`), no `javascript:` and no `data:`. Redirect targets from query parameters against an allowlist.
- **Code from strings:** no `eval`, `new Function` or `setTimeout` with a string.
- **Environment variables:** everything with the prefix from `envPrefix` (default `VITE_`) is public in the bundle. No secrets, API keys with write access or internal URLs there. Check `.env*` files by name only, never print values.
- **Tokens:** no auth tokens in `localStorage` or `sessionStorage`, any XSS can read them. Prefer `HttpOnly` cookies set by the backend.
- **Authorization:** checks in the frontend are for display only. Every protected action is checked by the backend.
- **postMessage:** check `event.origin` against an allowlist. Send with an explicit target origin, not `*`.
- **Third-party code:** scripts and iframes from external sources only when needed, with `sandbox` for iframes and Subresource Integrity for scripts from a CDN.
- **Content Security Policy:** check whether one is set and whether new code needs a weaker policy (inline scripts, `unsafe-eval`).
- **Source maps:** check `build.sourcemap` in the production build. Public source maps expose the source code.
- **Dependencies:** `npm audit` or the equivalent for the package manager. Packages with known vulnerabilities, unmaintained packages.
- **Error output:** no stack traces, internal IDs or API errors shown to users unfiltered.

## Report

By severity (critical, high, medium, low): `file:line`, problem, impact, fix proposal. Nothing found: say what was checked.
