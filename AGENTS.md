# AGENTS.md — pod-filebrowser

Standalone candy repo for the `filebrowser` candy — the FileBrowser Quantum web
file manager served on `8080`. The candy lives in `charly.yml` at the repo root
plus its server config `config.yaml`.

Canonical files:

- `charly.yml` — the `filebrowser:` candy entity (description, `require`, `port`,
  `volume`, `alias`, `var`, `service`, `plan`) and its `skill:` entity.
- `config.yaml` — the FileBrowser server config staged to
  `/etc/filebrowser/config.yaml`.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `CHANGELOG/` — per-CalVer history.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-filebrowser:filebrowser` — the owning skill: the service properties,
  the config, the volumes, and verification. Load before editing, building,
  deploying, or troubleshooting this candy.
- `/charly-pod:pod` — the `kind: pod` / deploy schema reference (this candy is
  composed into a box; tree-position nesting, volumes, ports).
- `/charly-check:check` — the check/R10 framework: the `check:` step verbs and
  `charly check run <bed>`.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs, `var:`, `alias:`, service declarations).
- `/charly-internals:git-workflow` — before any git/PR action.

## Build / validate / test

- `charly box validate` at the repo root — the structural check: the manifest
  must parse and validate at the installed charly.
- The live R10 witness is a composing box's `check` bed; the candy's own `check:`
  steps assert the installed binary, the config at
  `/etc/filebrowser/config.yaml` (including `port: 8080`), the running service,
  the HTTP `200` on the published port, and the mounted `data` volume.
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate. Its only workflow file is
  `.github/workflows/tag-on-merge.yml`.

## Modify this repo

- Edit the `filebrowser:` candy entity in `charly.yml`; the `skill:` entity in the
  same file is the owning skill's source — a candy change and its skill change
  land together.
- The pinned version is the `FILEBROWSER_VERSION` var; the server settings (port,
  database, sources) live in `config.yaml`. Keep the service, the `port:` field,
  and the config in step.
- The `data` and `files` volumes are the persistent stores; a change to their
  paths must move with the config's `database`/`cacheDir`/`sources`.
- The `skill:` entity is the source for `/charly-filebrowser:filebrowser`; never
  edit the generated `SKILL.md` — regenerate it.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
