---
name: showmeai
description: >
  Generate and edit images, create videos, convert images to 3D, synthesize speech or music,
  and process images through ShowMeAI. Use when a user asks an Agent to create media,
  configure ShowMeAI models, inspect token-group availability, or resume a generation task.
version: "2.0.0"
template: multi-scene
author: ian
homepage: https://api.showmeai.art
license: MIT
compatibility: Python 3.10+; macOS, Linux, and Windows; Agent-platform independent
triggers:
  - generate or edit an image
  - generate a video or convert an image to 3D
  - synthesize speech or music
  - upscale an image or remove its background
  - configure ShowMeAI Key, models, or generation defaults
  - inspect token-group model availability
  - resume an unfinished generation task
token_budget: 1800
metadata:
  category: creative-media
  api: ShowMeAI
---

# ShowMeAI Universal Media Skill

## Purpose

Give Agents one safe, deterministic interface for ShowMeAI creative-media generation, configuration, result download, and long-task recovery.

## Context

The Agent interprets creative intent and chooses a workflow. The Python runtime owns secrets, parameter validation, API payloads, retries, state transitions, downloads, and persistence. Do not reimplement those deterministic operations in prose or ad-hoc shell calls.

Use `python3 {baseDir}/scripts/showmeai.py`. Every command returns JSON; generated files are also emitted as `MEDIA:<absolute-path>`.

## Instructions

1. Before generation, run `doctor`. If it returns `SETUP_REQUIRED`, offer the one-time setup below. Never ask for a Key that is already configured.
2. Accept a Key only through hidden interactive input or `setup --key-stdin`; never put it in arguments, config JSON, logs, or replies.
3. Treat `models` as the current Key group's view. If a requested model is absent, tell the user to switch the API token group or enable automatic grouping, then refresh.
4. Respect saved defaults unless the user overrides them. The initial image preference is `gemini-3.1-flash-image`; setup may choose another visible model.
5. For video, 3D, music, and image-processing tasks, keep the command alive until terminal success or failure. Do not stop after a task ID. On interruption, preserve the journal and use `tasks resume`.
6. Return downloaded local files, not only remote URLs or task IDs.

## One-time setup

Local interactive setup:

```bash
python3 {baseDir}/scripts/showmeai.py setup
```

When the user sends a Key to a trusted Agent, start this command and write the Key to standard input without echoing it:

```bash
python3 {baseDir}/scripts/showmeai.py setup --key-stdin --json
```

The wizard validates the Key, fetches its group model list, filters creative categories, guides category defaults and model-specific parameters, and stores them in an OS-native directory. See [configuration.md](references/configuration.md).

## Route requests

| Intent | Command | Read when needed |
|---|---|---|
| Setup, diagnose, list/configure models | `setup`, `doctor`, `models`, `config` | [configuration.md](references/configuration.md) |
| Generate or edit an image | `image` | [image.md](references/image.md) |
| Generate video | `video` | [video.md](references/video.md) |
| Convert image to 3D | `3d` | [three-d.md](references/three-d.md) |
| Speech or music | `tts`, `music` | [audio.md](references/audio.md) |
| Upscale or remove background | `pic` | [image-tools.md](references/image-tools.md) |
| Long-running/recoverable task | `tasks list`, `tasks resume` | [polling.md](references/polling.md) |

Use `python3 {baseDir}/scripts/showmeai.py <command> --help` for exact flags. Legacy scripts remain compatibility wrappers.

## Output

```json
{"ok":true,"data":{"kind":"image","model":"gemini-3.1-flash-image","files":["/absolute/path/result.png"]}}
```

Failures use `{"ok":false,"error":{"code":"...","message":"...","retryable":false}}`. Relay the safe message and recovery action. For success, return all files and mention any fallback model actually used. Never expose secrets.

## Output file conventions

All media is downloaded below `output.directory` (default `./showmeai-output`) in a category subdirectory. Return every absolute path from `data.files` and every `MEDIA:` line. Never overwrite an existing file. Async state belongs in the OS-native state directory.

## Notes

- A task-ID response is not a completed media result.
- The saved user model overrides the initial recommendation.
- `verify_on_use` means cataloged but not discoverable through the current Key's `/v1/models` response.
- `verified_uncataloged` means a newly discovered creative model can be selected, but its special parameters must use API defaults until cataloged.
- Image `--count` is a 1–10 output contract. The runtime may use bounded parallel single-image calls, so report the physical request count and note that each request may be billed.
- A user-specified `--max-wait` is the only normal wall-clock cutoff; otherwise keep polling through nonterminal states.

## Files

See [README.md](README.md) for the annotated distribution tree covering entry points, shared modules, data, on-demand references, and tests.

## Further Reading

- [README.md](README.md) — installation, Agent handoff prompt, examples, and file tree
- [DESIGN.md](DESIGN.md) — architecture, alternatives, limitations, and decisions
- [CHANGELOG.md](CHANGELOG.md) — release history
