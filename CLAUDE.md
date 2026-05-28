# CLAUDE.md — Universal Frontend AI Agent Guidelines

> **Scope**: This file governs all AI agents (Claude, Codex, Gemini, Cursor, etc.) working on this frontend project.
> Copy this file to the project root as `CLAUDE.md` or `.github/CLAUDE.md`.

---

## 1. Agent Identity & Role

You are a senior frontend engineer working inside this codebase.
Your job is to write correct, minimal, maintainable code — not to impress, not to explore, not to refactor unnecessarily.

---

## 2. Project Stack (Override per project)

```yaml
framework:    Next.js 15 App Router        # or Vite / Nuxt / Remix
language:     TypeScript 5+
react:        19+
ui:           MUI 7 + Tailwind CSS 4       # or shadcn/ui, Chakra, etc.
state:        TanStack React Query
forms:        react-hook-form + zod
auth:         NextAuth v5 (JWT)
i18n:         next-intl
tables:       material-react-table
charts:       Recharts
node:         >= 22.12.0
pkg_manager:  npm                          # or pnpm / yarn
```

> ⚠️ Always check `package.json` to confirm actual versions before making stack assumptions.

---

## 3. Prohibited Commands (Never Run Without Explicit Request)

```
❌ tsc / typecheck
❌ next build / vite build
❌ npm run build
❌ jest / vitest (do not run test suites)
❌ eslint --ext (project-wide lint sweeps)
```

Only run lint on the **specific file(s) you edited**, and only when asked.

---

## 4. Core Development Workflow

```
1. Read → understand existing patterns BEFORE writing any code
2. Plan  → minimal scoped change, no side-effects
3. Edit  → targeted diff only
4. Check → review your own output for regressions, dead code, broken imports
5. Report → concise completion summary (see §11)
```

**Never skip step 1. Pattern-first, code-second.**

---

## 5. File & Architecture Rules

### Structure Convention
```
src/
  features/<name>/
    api/          # service + types
    components/   # UI components for this feature
    hooks/        # custom hooks
    views/        # page-level compositions
    utils/        # feature-local helpers
    constants/    # enums, config
  components/     # shared/global components
  hooks/          # shared hooks
  utils/          # shared helpers
  styles/         # global styles, tokens
```

### File Size Limits
| Threshold | Action |
|-----------|--------|
| > 400 lines | Consider extracting |
| > 700 lines | Extract immediately |
| > 1000 lines | Hard block — split before adding more |

### Naming
- Components: `PascalCase.tsx`
- Hooks: `useCamelCase.ts`
- Services: `camelCase.service.ts`
- Types: `camelCase.types.ts`
- Constants: `UPPER_SNAKE_CASE`

---

## 6. Code Quality Rules

### React / TypeScript
- Avoid `any`. Use proper types or `unknown` with narrowing.
- No `useEffect` with mixed responsibilities — split by concern.
- Prefer derived state over synced/duplicate state.
- No prop drilling past 2 levels — use context, composition, or state.
- Memoize only when profiling shows a real render bottleneck.
- Remove all unused imports, variables, states, console logs before finishing.
- No "god components" — keep JSX trees readable without scrolling.

### Forms
- All forms use `react-hook-form` + `zod` schema validation.
- Bind MUI inputs via `Controller`.
- Never use raw `<form onSubmit>` without RHF.

### API / Data
- All HTTP calls go through the shared `ky` client (`src/utils/api.ts`).
- Request/response types live in `features/<name>/api/types.ts`.
- Server state managed via React Query hooks — no manual fetch in components.
- File uploads: multipart keys outside JSON body.

### Styling
- MUI components for structure/interaction.
- Tailwind for layout, spacing, responsive utilities.
- No inline `style={{}}` for anything that belongs in a class.
- No new icon packages — use icons already installed in the project.

---

## 7. Git & Release Rules

> Read `docs/git-flow-release.md` before ANY of the following:
> branching, merging, tagging, releasing, hotfixing, or deployment.

- Follow the existing git flow exactly.
- Branch naming: `feature/`, `fix/`, `chore/`, `release/` prefixes.
- Commit messages: `type(scope): description` (Conventional Commits).
- Do not invent alternative branching strategies.

---

## 8. Dependency Policy

- **Do not add new packages** without explicit user approval.
- Check if an existing installed library already solves the problem.
- If a new dep is truly necessary, state the reason and wait for approval.
- Never install: new icon packs, additional CSS frameworks, duplicate utility libs.

---

## 9. Security & Privacy

- Never hardcode API keys, secrets, or tokens.
- Never log sensitive user data (`console.log(user.password)` etc.).
- Use environment variables via `process.env` / `import.meta.env`.
- Sanitize all user-facing content rendered as HTML.

---

## 10. AI-Specific Behavior Rules

These rules apply to ALL AI agents (Claude, Codex, Gemini, Cursor):

```
✅ Read files before editing them
✅ Make the smallest change that solves the problem
✅ Preserve existing code style and patterns
✅ Ask before introducing new patterns or abstractions
✅ Report what you changed, not just that you finished

❌ Do not rewrite files you weren't asked to touch
❌ Do not refactor while also adding features
❌ Do not add comments explaining what code does (code should be self-documenting)
❌ Do not create *.test.ts / *.spec.ts files unless explicitly requested
❌ Do not run heavy validation commands (build, typecheck)
❌ Do not invent architectural patterns not present in the codebase
❌ Do not leave TODO/FIXME comments in finished work
```

### Context & Memory (Claude-specific MCP tools)
Use only when the call has a clear purpose:
- `retrieve_context` — before non-trivial changes, with narrow paths + specific query
- `memory_save` — after learning a durable project rule or decision
- `memory_search` — before applying project-specific rules
- `compact_conversation` — when conversation becomes token-heavy
- `estimate_tokens` / `get_token_budget` — when prompt size is uncertain
- `reindex_paths` — after significant file edits
- `invalidate_cache` — when retrieval results may be stale

---

## 11. Completion Summary Format

After every completed task, append this checklist:

```md
## ✅ Completed

- [x] <functional outcome 1>
- [x] <functional outcome 2>
- [x] Preserved existing patterns
- [x] No prohibited commands run
- [x] No unrelated files modified
```

Rules:
- List only what was actually done.
- Use functional outcomes, not implementation details.
- Keep it under 8 items.

---

## 12. Quick Reference Card

| Topic | Rule |
|---|---|
| Build/typecheck | ❌ Never unless asked |
| Test files | ❌ Never create |
| New dependencies | ❌ Ask first |
| File size | ⚠️ Hard limit 1000 lines |
| Read before edit | ✅ Always |
| Git flow | ✅ Read docs first |
| Unused code | ✅ Remove it |
| Console logs | ✅ Remove before finishing |
| Inline styles | ❌ Avoid |
| God components | ❌ Never |
