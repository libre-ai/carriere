# Carriere Canonical Agent Rules

## Purpose

Reserved couche-1 product home for Carrière, a sovereign and explainable
job-search assistant for executives: application tracking where every
recommendation can be explained. No specification or code exists yet — the
scope awaits owner clarification (`project.v1.yaml`, maturity `idea`).
Doctrine lives upstream: https://raw.githubusercontent.com/libre-ai/governance/main/docs/README.md

## Domain doctrine

- Sovereignty constraints from the project card: no hosting outside the
  European Union, no opaque scoring — recommendations stay explainable.
- No commitment until the owner clarifies the scope: do not write a
  specification or product code here before that clarification.
- `project.v1.yaml` is the authority on project state; the "État du projet"
  README section is generated from it — never edit that section by hand.
- No bricks or contracts are consumed yet (`dependencies: []` in the card);
  when they arrive they are consumed pinned, never redefined here.

## Commands

- Spec-only repository: no manifest, no build, no test suite — tracked
  content is `project.v1.yaml`, `README.md` and `README.fr.md`.
- The only CI gate is the `Context hygiene` workflow
  (`.github/workflows/context-hygiene.yml`), run on every push and pull
  request: it blocks private identifiers and machine-local paths.
- Read the full diff before pushing: `git diff origin/main` — run before pushing.

## Working here

- Security > quality > performance > completeness, in that order on conflict.
- Check real state before editing: `git status --short` (no test suite exists).
- English for code, comments and this file; French stays the human
  conversation language elsewhere.
- Never commit a machine-local absolute filesystem path; use repo-relative
  paths or `~` instead.
