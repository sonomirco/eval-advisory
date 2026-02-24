# Repository Guidelines

## Project Structure & Module Organization
This repository is a documentation-first skill package.

- `eval-advisor/SKILL.md`: behavior contract and workflow policies.
- `EVAL_MASTER.md`: routing index for all 12 workflows.
- `eval-advisor/references/`: conceptual source files (`*-guide.md`, `*-reference.md`).
- `eval-advisor/workflows/`: execution checklists (`*-checklist.md`).
- `eval-advisor/templates/`: artifact schemas (`*-template.md`, prompt templates).
Keep theory in `eval-advisor/references/`, procedures in `eval-advisor/workflows/`, and output formats in `eval-advisor/templates/`.

## Build, Test, and Development Commands
There is no build pipeline or runtime app in this repository. Use lightweight doc validation:

- `rg --files` — list tracked files quickly.
- `rg -n "pattern" eval-advisor/` — find and update related guidance.
- `sed -n '1,200p' path/to/file.md` — inspect files in small chunks.

## Coding Style & Naming Conventions
- Use concise Markdown with clear headings and short sections.
- Prefer kebab-case filenames with functional suffixes:
  - `*-guide.md`, `*-reference.md`, `*-checklist.md`, `*-template.md`.
- Preserve the no-duplication contract in `SKILL.md`.

## Testing Guidelines
No automated test framework is configured. Treat review as content QA:

- Verify routing still maps request domain to the correct primary reference.
- Check workflows stay procedural and templates stay artifact-only.
- Confirm internal links and file paths resolve.

## Commit & Pull Request Guidelines
Use a clear, conventional style:

- Commit format: imperative, specific subject line (example: `Refine error-analysis checklist quantification gates`).
- PRs should include:
  - scope summary,
  - files changed and why,
  - confirmation that no-duplication and reference-first rules were preserved,
  - screenshots only for rendering/layout changes.

## Agent-Specific Instructions
### Progressive Disclosure Loading Model (Mandatory)
1. **Level 1 (Always load):** `eval-advisor/SKILL.md` and `EVAL_MASTER.md`.
2. **Level 2 (Only when needed):** module-specific execution instructions (`eval-advisor/workflows/*` and matching workflow section in `EVAL_MASTER.md`).
3. **Level 3 (Only when required):** detailed concept files (`eval-advisor/references/*`) and artifact files (`eval-advisor/templates/*`) selected by `EVAL_MASTER.md`.

### Routing and Source-of-Truth Rules
- First classify domain using `EVAL_MASTER.md`, then load only that path.
- Load order: primary `reference -> workflow -> template (if needed)`.
- Use secondary references only if the selected workflow row lists them.
- Never treat `SKILL.md`, `workflows/`, or `templates/` as conceptual source-of-truth when a matching reference exists.

### Response Hygiene
1. Confirm conceptual claims came from the selected primary reference.
2. Remove duplicated theory that belongs in `references/`.
3. Keep workflow/template outputs procedural and concise.
