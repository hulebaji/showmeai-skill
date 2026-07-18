# Configuration and credentials

Read this reference for setup, Key errors, model visibility, or default changes. Routing rules remain in `SKILL.md`.

## Setup contract

`setup` must validate the Key with `GET /v1/models` before saving it. Interactive setup uses hidden input. Agent-assisted setup uses `--key-stdin`; the Agent writes the secret to the child process and must not echo it, put it in an argument, or retain it in a reply.

After validation, the wizard groups only image, video, 3D, TTS, STT, music, and image-tool capabilities. It then offers defaults for callable generation categories and their `basic` parameters from `data/model-catalog.json`.

## Group-aware discovery

The API model list is scoped to the Key's token group. A missing model is not evidence that ShowMeAI does not support it. Tell the user to switch token groups or enable automatic grouping in the ShowMeAI console, then rerun `models` or `setup`.

Entries marked `verified` were returned by `/v1/models`. Cataloged task APIs that cannot be discovered there may be shown as `verify_on_use`.

New creative model IDs recognized from the live response but absent from the bundled parameter catalog are shown as `verified_uncataloged`. They can be selected, but the Agent must not invent model-specific parameters; use API defaults until the catalog is updated.

## Resolution order and paths

Key resolution order:

1. `SHOWMEAI_API_KEY`
2. legacy `Showmeai_API_KEY`
3. OS-native `credentials` file

The non-secret `config.json` and secret `credentials` file live under:

- macOS: `~/Library/Application Support/ShowMeAI Skill/`
- Linux: `${XDG_CONFIG_HOME:-~/.config}/showmeai-skill/`
- Windows: `%APPDATA%\ShowMeAI Skill\`

The task journal uses the corresponding state directory. `SHOWMEAI_CONFIG_DIR`, `SHOWMEAI_CONFIG_FILE`, and `SHOWMEAI_STATE_DIR` are portable overrides.

## Commands

```bash
python3 scripts/showmeai.py setup
python3 scripts/showmeai.py setup --key-stdin --json
python3 scripts/showmeai.py setup --replace-key
python3 scripts/showmeai.py doctor
python3 scripts/showmeai.py models
python3 scripts/showmeai.py config show
python3 scripts/showmeai.py config set defaults.image.model gemini-3.1-flash-image
```

`config show` exposes only a Key fingerprint. Never print the credentials file.
