# notebook-ollama

The Ollama integration notebook collection as a charly *data layer* — 6 Jupyter
notebooks demonstrating every way to talk to a local Ollama server, seeded into
the workspace volume of a Jupyter image at deploy time.

The `notebook-ollama` candy ships no packages, no services and no dependencies.
Its whole job is to stage `data/ollama/` into the `workspace` volume's `ollama/`
subdirectory, so a freshly deployed JupyterLab pod has runnable Ollama examples
on first open. It pairs naturally with the `ollama` server layer for a
self-contained local LLM stack.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `notebook-ollama` |
| Type | Data-only — no packages, no services, no dependencies |
| Volume | `workspace` → `/workspace` (supplied by the jupyter base) |
| Data | `data/ollama` → `workspace` volume, dest `ollama` |
| Notebooks | 6 `.ipynb` files + `notebooks.yaml` |

## Notebook contents

| Notebook | Client library | Features |
|---|---|---|
| `00_Ollama_Requests.ipynb` | `requests` | Raw REST API: list, show, generate, chat, stream, embed, copy, delete |
| `01_Ollama_GPU.ipynb` | `requests` | GPU verification: `nvidia-smi`, inference metrics, VRAM monitoring |
| `02_Ollama_OpenAI.ipynb` | `openai` | OpenAI-compatible API: completions, chat, multi-turn, stream, embed |
| `03_Ollama_Library.ipynb` | `ollama` | Native Python library: all API operations + model management |
| `04_Ollama_HuggingFace.ipynb` | `ollama` | HuggingFace GGUF model import, verification, inference |
| `05_Ollama_Anthropic.ipynb` | `anthropic` | Anthropic-compatible API: chat, system prompts, streaming, tool calling, vision |

The notebooks read `OLLAMA_HOST` (default `http://localhost:11434`). When the
`ollama` box is deployed via `charly config ollama --update-all`, its
`env_provide` injects `OLLAMA_HOST=http://charly-ollama:11434` into every
container on the same `charly` network — no manual environment setup.

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list, e.g. `jupyter-ml-notebook`:

```yaml
jupyter-ml-notebook:
  candy:
    base: fedora-nonfree
    candy:
      - '@github.com/opencharly/layer-notebook-ollama:v2026.239.1601'
      # ... other notebook data layers
```

Then:

```bash
charly config ollama --update-all   # deploys ollama + propagates OLLAMA_HOST
charly start ollama
charly start jupyter-ml-notebook    # OLLAMA_HOST already set
# Open http://localhost:8888 -> navigate to ollama/
```

## Layout

- `charly.yml` — the `notebook-ollama:` candy entity (the `data:` mapping and the
  `plan:` checks) plus the embedded `skill:` entity.
- `data/ollama/` — the 6 notebooks and `notebooks.yaml`.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-jupyter:notebook-ollama` — the notebooks, the Ollama
  client-library gotchas, and the connection story.
- Server: `/charly-ollama:ollama`.
- Sibling data layers: `/charly-jupyter:notebook-llm-on-supercomputers`,
  `/charly-jupyter:notebook-openrouter`, `/charly-jupyter:notebook-templates`.
- Consuming box: `/charly-jupyter:jupyter-ml-notebook`.
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella.
