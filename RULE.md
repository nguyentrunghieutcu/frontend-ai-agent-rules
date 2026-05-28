# RULE.md — Universal Frontend Development Rules

> **Applies to**: All AI agents and human contributors on this project.
> **Compatible with**: Claude, Codex, Gemini, Cursor, Windsurf, Continue, Aider.
> Place this at `.claude/rules/RULE.md` or `docs/RULE.md`.

---

## PART 1 — ABSOLUTE PROHIBITIONS

These are never negotiable. No exceptions.

### 1.1 Forbidden Commands
```
❌ tsc --noEmit         (typecheck)
❌ next build           (production build)
❌ vite build
❌ npm run build
❌ jest / vitest        (test runner)
❌ eslint (project-wide sweep)
```

### 1.2 Forbidden File Actions
```
❌ Create *.test.ts / *.spec.ts / *.test.tsx files
❌ Modify files outside the task scope
❌ Revert unrelated working-tree changes
❌ Delete files without explicit instruction
❌ Hardcode secrets, API keys, or tokens
```

### 1.3 Forbidden Code Patterns
```
❌ any (TypeScript escape hatch)
❌ console.log / console.error left in finished code
❌ Unused imports, variables, or state
❌ TODO / FIXME comments in delivered code
❌ Inline style={{ }} for layout/spacing concerns
❌ Direct fetch() calls — use the shared API client
❌ New icon packages — use existing ones only
❌ New CSS frameworks — use existing stack only
```

---

## PART 2 — MANDATORY WORKFLOWS

### 2.1 Before Editing Code
```
1. Read the target file(s) fully
2. Identify the existing pattern being used
3. Plan the minimal change
4. Confirm you're not touching unrelated files
```

### 2.2 Before Adding Dependencies
```
1. Check if existing dependencies already solve it
2. State the need explicitly to the user
3. Wait for approval before installing
```

### 2.3 Before Git Operations
```
1. Read docs/git-flow-release.md
2. Follow the documented flow exactly
3. Never invent branch names or merge strategies
```

### 2.4 When Modifying Large Files (> 400 lines)
```
1. Assess if the file needs extraction before adding more code
2. If > 700 lines: extract first, then add
3. If > 1000 lines: hard stop — split the file before proceeding
```

---

## PART 3 — CODE QUALITY STANDARDS

### 3.1 Component Rules
- One responsibility per component.
- JSX tree must be readable without horizontal scrolling.
- No deeply nested ternaries — extract to variables or sub-components.
- No prop drilling past 2 levels.
- Composition over configuration.

### 3.2 Hook Rules
- One purpose per hook.
- No mixed `useEffect` blocks (side effects must be separated by concern).
- Prefer derived state over `useState` + sync logic.
- Cleanup effects always when subscribing to events or timers.

### 3.3 Service / API Rules
- All HTTP calls through the shared client (`src/utils/api.ts` or equivalent).
- Types defined in `features/<name>/api/types.ts`.
- React Query for all server state — no manual fetch in components.
- Error handling at the service layer, not scattered across components.

### 3.4 Form Rules
- `react-hook-form` + `zod` for all forms.
- MUI inputs wrapped in `Controller`.
- Validation schema defined separately from the component.
- Submit handler outside JSX.

### 3.5 Styling Rules
- MUI for component structure/behavior.
- Tailwind for layout, spacing, responsive.
- CSS variables for design tokens.
- No new utility class libraries.
- Responsive-first — test mobile layout before desktop.

---

## PART 4 — ARCHITECTURE PRINCIPLES

### 4.1 Feature-First Structure
```
src/features/<feature-name>/
  api/          → HTTP service + TypeScript types
  components/   → UI components scoped to this feature
  hooks/        → Custom hooks for this feature
  views/        → Page-level composition components
  utils/        → Feature-local helpers
  constants/    → Enums, static config
```

### 4.2 Shared Code
```
src/
  components/   → Truly reusable cross-feature UI
  hooks/        → Shared custom hooks
  utils/        → Shared utilities
  styles/       → Global styles, design tokens
  types/        → Shared TypeScript interfaces
```

### 4.3 Separation of Concerns
| Layer | Responsibility |
|---|---|
| `views/` | Page orchestration only — no business logic |
| `components/` | Render + local UI state only |
| `hooks/` | Data fetching, derived state, side effects |
| `services/` | API calls, data transformation |
| `utils/` | Pure functions, no side effects |

---

## PART 5 — PERFORMANCE RULES

- No inline object/array/function creation in render for list items.
- Memoize (`useMemo`, `useCallback`) only when a measured render bottleneck exists.
- Use dynamic `import()` only when it meaningfully reduces initial bundle.
- Avoid re-renders caused by context value reference changes.
- Virtualize lists > 100 items (use existing virtualization lib if installed).

---

## PART 6 — SECURITY RULES

- No secrets in source code — use environment variables.
- Sanitize all HTML rendered from user input or API responses.
- Never log user PII (email, passwords, tokens) to console.
- Validate all form inputs server-side too — client validation is UX only.
- Use `rel="noopener noreferrer"` on all `target="_blank"` links.

---

## PART 7 — AI AGENT BEHAVIORAL RULES

These apply regardless of which AI model is running:

### Do
- Read before writing.
- Ask when uncertain about scope or pattern.
- Report what changed and why in the completion summary.
- Prefer the existing pattern over introducing a new one.
- Keep diffs minimal and reviewable.

### Do Not
- Silently modify files outside the stated task.
- Mix feature additions with refactors in one change.
- Generate boilerplate that wasn't asked for.
- Add comments that explain what the code obviously does.
- Create placeholder/mock data that ends up in the final codebase.
- Leave half-finished implementations without flagging them.

---

## PART 8 — COMPLETION CHECKLIST

Run through this before declaring a task done:

```
Code Quality
  [ ] No unused imports or variables
  [ ] No console.log statements
  [ ] No TypeScript any without justification
  [ ] No TODO/FIXME left in delivered code
  [ ] File size within limits (< 700 lines preferred)

Architecture
  [ ] Change is scoped to the correct feature folder
  [ ] No new patterns introduced without discussion
  [ ] Existing utilities reused where applicable

Rules Compliance
  [ ] Did NOT run build/typecheck
  [ ] Did NOT create test files
  [ ] Did NOT install new dependencies (or got approval)
  [ ] Did NOT modify unrelated files

Output
  [ ] Completion summary written
  [ ] Changed files listed
  [ ] Any caveats or follow-up items surfaced
```

---

## PART 9 — TASK COMPLETION SUMMARY FORMAT

```md
## ✅ Completed

- [x] <Functional outcome — what the user can now do>
- [x] <Functional outcome 2>
- [x] Preserved existing patterns and architecture
- [x] No prohibited commands executed
- [x] No unrelated files modified

**Files changed:**
- `src/features/x/components/Y.tsx` — <one-line reason>
- `src/features/x/hooks/useY.ts` — <one-line reason>

**Notes / Follow-up (if any):**
- <Any edge cases, assumptions, or things to revisit>
```
