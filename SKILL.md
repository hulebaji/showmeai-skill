---
name: Showmeai
description: Generate images and videos via Showmeai API. Image gen uses OpenAI-compatible Images API (nano-banana and gpt-image models). Video gen uses Seedance API (doubao-seedance-1-5-pro). Images are NOT saved locally by default (URL only). Use --save flag when the user wants to keep the image. Videos are saved when available.
homepage: https://api.showmeai.art
license: MIT
metadata:
  {
    "openclaw":
      {
        "requires": { "bins": ["python3"], "env": ["Showmeai_API_KEY", "Showmeai_BASE_URL"] },
        "primaryEnv": "Showmeai_API_KEY",
      },
  }
---

# Showmeai Image & Video Generation

Generate images via Showmeai's OpenAI-compatible Images API (`/images/generations`) or videos via Seedance API (`/task/volces/seedance`).

## Basic Usage

```bash
python3 {baseDir}/scripts/gen.py --prompt "your prompt here"
```

## Options

```bash
# Specify model (default: nano-banana-pro)
python3 {baseDir}/scripts/gen.py --prompt "..." --model nano-banana-pro

# Higher resolution (append -2k or -4k to model name)
python3 {baseDir}/scripts/gen.py --prompt "..." --model nano-banana-pro-2k

# Save image locally (default: NO save, URL only)
python3 {baseDir}/scripts/gen.py --prompt "..." --save

# Save to OSS directory (~/.openclaw/oss/)
python3 {baseDir}/scripts/gen.py --prompt "..." --oss

# Save to custom directory
python3 {baseDir}/scripts/gen.py --prompt "..." --save --out-dir /path/to/dir

# Aspect ratio
python3 {baseDir}/scripts/gen.py --prompt "..." --aspect-ratio 16:9

# Image count
python3 {baseDir}/scripts/gen.py --prompt "..." --count 2

# Edit image (provide --input)
python3 {baseDir}/scripts/gen.py --prompt "make it look like a painting" --input /path/to/image.jpg
```

## Video Generation

```bash
# Basic text-to-video
python3 {baseDir}/scripts/video_gen.py --prompt "A detective enters a dim room, examines clues on the desk"

# With reference image (image-to-video)
python3 {baseDir}/scripts/video_gen.py --prompt "Girl holding a fox, opens eyes and looks at camera gently" --image /path/to/image.jpg

# First-and-last-frame video (requires both frames)
python3 {baseDir}/scripts/video_gen.py --prompt "A blue-green bird transforms into human form" --first-frame /path/to/first.jpg --last-frame /path/to/last.jpg

# Without audio (cheaper)
python3 {baseDir}/scripts/video_gen.py --prompt "..." --no-audio

# Draft/preview mode (faster, lower quality)
python3 {baseDir}/scripts/video_gen.py --prompt "..." --draft

# Save to local directory
python3 {baseDir}/scripts/video_gen.py --prompt "..." --save --out-dir /path/to/dir
```

## Supported Models

nano-banana series (returns URL, fast):
- nano-banana
- nano-banana-pro  ← default
- nano-banana-2
- nano-banana-pro-2k / nano-banana-pro-4k (high res)

gpt-image series (returns base64, always saved):
- gpt-image-1
- gpt-image-1.5

Video models (Seedance API):
- doubao-seedance-1-5-pro-251215 ← default (supports audio, draft mode, text-to-video, image-to-video, first-and-last-frame)

## Config

Set in `.env` or `~/.openclaw/openclaw.json`:
- `Showmeai_API_KEY` — your Showmeai API key **(required)**
- `Showmeai_BASE_URL` — base URL with /v1 suffix **(required)**; defaults to `https://api.showmeai.art/v1` if not set

## Save Behavior

**Images:**
- Default: no local file, output `MEDIA:<url>` directly
- `--save`: save to `~/.openclaw/media/`
- `--oss`: save to `~/.openclaw/oss/`
- gpt-image models always save to media/ (API returns base64 only)

**Videos:**
- Videos are saved when the API returns direct URL or base64 data
- For async task submission, the task ID is output
- Use `--save` to ensure local saving when video is available

## Video Prompt Parameters

Can append parameters to prompt text:
- `--ratio 16:9` / `--ratio adaptive` — aspect ratio
- `--rs 720p` / `--rs 480p` — resolution (720p, 480p available; 1080p coming soon)
- `--dur 5` / `--dur 10` — duration in seconds (5s or 10s)
- `--cf false` — disable fixed camera

Video specs: 24 FPS, durations 5s/10s, resolutions 480P/720P/1080P (coming soon).
