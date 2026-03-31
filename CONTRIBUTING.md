# Contributing Guide

Thank you for contributing to this repository. Please follow these conventions to keep the history clean and collaboration smooth.

---

## Branching

**Never commit directly to `main`.** Always work on a dedicated branch.

1. Pull the latest `main`:
   ```
   git checkout main
   git pull origin main
   ```
2. Create a branch using one of these prefixes:
   | Prefix | When to use |
   |--------|-------------|
   | `feature/<short-description>` | New content or functionality |
   | `docs/<short-description>` | Documentation additions or updates |
   | `fix/<short-description>` | Corrections to existing content |
   | `chore/<short-description>` | Tooling, config, or housekeeping |

   Examples: `docs/add-risks-section`, `fix/timeline-dates`

---

## Commit Messages

Use **Conventional Commits** format:

```
type(scope): short description
```

- **Types:** `feat`, `fix`, `docs`, `chore`, `refactor`, `test`
- **Scope:** the area being changed (e.g., `prd`, `readme`, `config`)
- **Subject:** ≤ 72 characters, imperative mood ("add", "update" — not "added", "updated")
- One logical change per commit

Good examples:
```
docs(prd): add risks section with likelihood/impact matrix
fix(prd): correct CSR adoption percentage to 60%
chore: add CONTRIBUTING.md
```

---

## Before You Commit

For Markdown documents, check:

- Headings follow the existing structure (H1 → H2 → H3)
- Numbers, dates, and names are consistent throughout the document
- Any links or references resolve correctly
- The document's status field is up to date if its state changed

---

## Merging to Main

- Submit a **Pull Request** — do not push directly to `main`
- Keep PRs scoped to a single concern
- Prefer **squash merge** for small branches
- Delete your branch after it is merged
- Use a PR title that matches your primary commit message format

---

## Questions?

If you're unsure about scope or approach, open an issue or leave a comment on the relevant PR before making changes.
