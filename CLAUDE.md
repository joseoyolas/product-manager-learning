# Claude Code Instructions

This file governs how Claude Code behaves in this repository. These rules apply to every session.

---

## Branching

- **Never commit directly to `main`.**
- Always branch from the latest `main`:
  ```
  git checkout main && git pull origin main && git checkout -b <branch-name>
  ```
- Branch naming convention:
  | Prefix | When to use |
  |--------|-------------|
  | `feature/<short-description>` | New content or functionality |
  | `docs/<short-description>` | Documentation additions or updates |
  | `fix/<short-description>` | Corrections to existing content |
  | `chore/<short-description>` | Tooling, config, or housekeeping |

  Examples: `docs/add-risks-section`, `fix/timeline-dates`, `chore/add-claude-md`

---

## Commits

Use **Conventional Commits** format for every commit message:

```
type(scope): short description
```

- **Types:** `feat`, `fix`, `docs`, `chore`, `refactor`, `test`
- **Scope:** the area being changed (e.g., `prd`, `readme`, `config`)
- **Subject line:** ≤ 72 characters, imperative mood ("add", "update", "fix" — not "added", "updated")
- One logical change per commit — no "WIP" commits on shared branches

Examples:
```
docs(prd): add risks section with likelihood/impact matrix
fix(prd): correct CSR adoption percentage to 60%
chore: add CLAUDE.md with git workflow rules
```

---

## Validation Before Committing

Since this is a documentation repository, run these checks before committing:

1. **Markdown structure** — confirm headings follow the existing document style (H1 title → H2 sections → H3 subsections).
2. **Internal consistency** — cross-check any numbers, dates, or names mentioned in multiple places.
3. **No broken references** — if links or file references exist, verify they resolve.
4. **Status field** — if a PRD status changes (e.g., Draft → In Review), confirm it is reflected in the document header.

---

## Merging to Main

- **Never push feature work directly to `main`.**
- Merge only via Pull Request.
- Prefer **squash merge** for small, focused branches to keep history clean.
- Delete the branch after it is merged.
- PR title must match the primary commit message format.
- **Do not create a PR unless the user explicitly asks for one.**

---

## General Hygiene

- Keep PRs scoped to a single concern.
- Do not amend published commits — create a new commit instead.
- Do not force-push to `main` under any circumstances.
- When in doubt about scope or approach, ask the user before acting.
