# AGENTS.md — layer-notebook-ollama

Standalone candy repo for the `notebook-ollama` data layer — 6 Jupyter notebooks
demonstrating Ollama integration (raw REST, OpenAI-compatible, native library,
HuggingFace import, Anthropic-style, GPU), seeded into the workspace volume of a
Jupyter image at deploy time. The candy lives in `charly.yml` at the repo root:
the `data:` mapping into the `workspace` volume, the `plan:` `check:`
assertions, and the embedded `skill:` entity projected into the marketplace
corpus as `/charly-jupyter:notebook-ollama`.

Canonical files:

- `charly.yml` — the `notebook-ollama:` candy entity and the
  the embedded `skill:` entity.
- `data/ollama/` — the notebooks and `notebooks.yaml`.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-jupyter:notebook-ollama` — the owning skill. The notebook catalog, the
  `ollama` Python library env-var and Pydantic gotchas, and how notebooks connect
  to a separate Ollama container. Load before editing or troubleshooting.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, the `data:` field, and service
  declarations). Load before editing any entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence — they assert
  the data subdirectory is provisioned, the producer notebook is present, and a
  substantial collection is staged. A change to the collection must keep those
  checks honest.

## Modify this repo

- Edit the `notebook-ollama:` candy entity AND the embedded `skill:` entity in `charly.yml` together. The skill is the projected usage source,
  so a data, path, or behaviour change not mirrored in the skill leaves the
  corpus stale.
- The `data:` `dest:` field places the notebooks in a volume subdirectory rather
  than the volume root; keep it consistent with the `plan:` checks and the skill.
- New behaviour claims belong in the `plan:` as an observable `check:` step, and
  in the skill body.

## Landing

- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo — read it
  before landing.
- Release history lives in `CHANGELOG/`.
