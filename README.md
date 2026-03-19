# 🎨 Showmeai

Generate **images and videos** with AI via Showmeai's API.
通过 Showmeai API，让你的 AI 助手学会画图和生成视频。

Image generation uses OpenAI-compatible Images API. Video generation uses Seedance (Doubao) API. Fast, flexible, works out of the box once configured.

---

## ✅ Requirements / 前置条件

- Python 3
- A [Showmeai API Key](https://api.showmeai.art) (`Showmeai_API_KEY`)

---

## ⚙️ Configuration / 配置

Set in `.env` or `~/.openclaw/openclaw.json`:
- `Showmeai_API_KEY` — your Showmeai API key **(required)**
- `Showmeai_BASE_URL` — base URL with /v1 suffix **(required)**; defaults to `https://api.showmeai.art/v1` if not set

---

## 💬 Example Prompts / 示例提示词

Once configured, just tell your AI assistant what you want:

配置好后，直接用自然语言告诉 AI 你想要什么：

### 🖼️ Basic Generation / 基础生图

| English | 中文 |
|---|---|
| Draw a sunset over the ocean | 画一张海上日落的风景 |
| Generate a cyberpunk city at night | 生成一张赛博朋克风格的城市夜景 |
| Create a cute cartoon cat illustration | 画一只可爱的卡通猫咪插画 |

### 📐 With Aspect Ratio / 指定比例

| English | 中文 |
|---|---|
| Generate a 16:9 wallpaper of a starry sky | 生成一张 16:9 的星空壁纸 |
| Draw a 2:3 portrait of a fantasy warrior | 画一张 2:3 竖版奇幻战士肖像 |
| Create a 1:1 square avatar of a fox in a suit | 画一个穿西装的狐狸头像，1:1 方形 |

### 💾 Save to Local / 保存到本地

| English | 中文 |
|---|---|
| Generate a forest scene and save it | 生成一张森林场景的图片，保存到本地 |
| Draw 3 variations of a mountain landscape and save | 画 3 张山景图，保存到本地 |

### 🔍 High Resolution / 高清生图

| English | 中文 |
|---|---|
| Generate a high-res 4K illustration of a dragon | 用高清 4K 模型画一条龙的插画 |
| Create a 2K detailed city map illustration | 生成一张 2K 高清城市地图插画 |

---

## 🎬 Video Generation / 视频生成

### Text-to-Video / 文生视频

| English | 中文 |
|---|---|
| Generate a video of a detective entering a dim room | 生成一段侦探进入昏暗房间的视频 |
| Create a video of a cat playing with a ball of yarn | 生成一段猫咪玩毛线球的视频 |
| Make a 10 second video of a sunset timelapse | 制作一段 10 秒的日落延时视频 |

### Image-to-Video / 图生视频

| English | 中文 |
|---|---|
| Animate this image with the girl opening her eyes | 让这张图片里的女孩睁开眼睛动起来 |
| Create a video from this photo, camera slowly pulling out | 用这张照片生成视频，镜头缓慢拉出 |

### Video Parameters / 视频参数

| English | 中文 |
|---|---|
| Generate a 16:9 widescreen video | 生成 16:9 宽屏视频 |
| Create a 5 second video at 720p resolution | 生成 5 秒 720p 分辨率的视频 |
| Generate video without audio to save cost | 生成无声视频以节省费用 |

---

## 🤖 Supported Models / 支持的模型

### Image Models / 图像模型

| Model | Quality | Output |
|---|---|---|
| `nano-banana` | Standard | URL |
| `nano-banana-pro` ⭐ | Better — **Default** | URL |
| `nano-banana-2` | Gen 2 | URL |
| `nano-banana-pro-2k` | High-res 2K | URL |
| `nano-banana-pro-4k` | Ultra 4K | URL |
| `gpt-image-1` | High quality | Saved file |
| `gpt-image-1.5` | Higher quality | Saved file |

### Video Models / 视频模型

| Model | Features |
|---|---|
| `doubao-seedance-1-5-pro-251215` ⭐ | **Default**. Text-to-video, image-to-video, first-and-last-frame. Supports audio generation, draft mode. 24 FPS, 5s/10s duration, 480P/720P resolution. |

---

## 📁 Save Behavior / 图片保存说明

| Flag | Behavior |
|---|---|
| *(default)* | Returns image URL only, no local file |
| `--save` | Saves to `~/.openclaw/media/` |
| `--oss` | Saves to `~/.openclaw/oss/` |
| `gpt-image` models | Always saved (API returns base64 only) |

---

## 🔗 Links

- Showmeai API: [api.showmeai.art](https://api.showmeai.art)
- OpenClaw: [openclaw.ai](https://openclaw.ai)
- ClawHub: [clawhub.ai](https://clawhub.ai)
