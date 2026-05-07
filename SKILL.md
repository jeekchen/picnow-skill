---
name: picnow
description: >
  Generate high-quality images via Picnow (api.letmego.top). Activate when
  the user wants to create images, product photos, posters, illustrations,
  or any visual content — including editing or restyling an existing image.
  Requires LETMEGO_API_KEY environment variable (your letmego 令牌/token).
when_to_use:
  - User asks to generate, draw, or create an image
  - User says 生图 / 画图 / 画张 / 生成图片 / 帮我画
  - User wants a product photo, poster, banner, or illustration
  - User wants to edit or restyle an existing image (image-to-image)
allowed-tools:
  - Bash
---

# Picnow Image Generation Skill

Picnow wraps the `api.letmego.top` GPT-Image-2 API. This skill handles
text-to-image and image-to-image (edit) generation.

## 1. Environment check

Before generating, verify the token is set:

```bash
if [ -z "$LETMEGO_API_KEY" ]; then
  echo "❌ LETMEGO_API_KEY is not set."
  echo "Visit https://picnow.letmego.top/settings to get your 令牌, then:"
  echo "  export LETMEGO_API_KEY=your_令牌"
  exit 1
fi
```

## 2. Gather parameters

Ask the user if not already provided:

| Parameter | Options | Default |
|-----------|---------|---------|
| `prompt` | any text description | *(required)* |
| `quality` | `1k` · `2k` · `4k` | `1k` |
| `aspect` | `square` · `landscape` · `portrait` | `square` |
| `n` | 1 – 10 | `1` |
| `ref` | local file path (optional) | *(none → text-to-image)* |

**Quality guide:**
- `1k` — up to 1536×1536, fast and cheap, great for previews and social media
- `2k` — up to 2048×2048, suitable for e-commerce and print
- `4k` — up to 3840×2160, landscape or portrait only (no 4k square)

## 3. Locate and run the generation script

```bash
# Resolve skill directory (user-global → project-local)
SKILL_DIR="$HOME/.claude/skills/picnow"
[ -d "$SKILL_DIR" ] || SKILL_DIR=".claude/skills/picnow"

node "$SKILL_DIR/scripts/generate.js" \
  --prompt "PROMPT_HERE" \
  --quality QUALITY \
  --aspect ASPECT \
  --n N
  # add: --ref /path/to/image.jpg   for image-to-image
```

The script outputs a single JSON line:
```json
{ "urls": ["https://...cdn.../image.png"] }
```

## 4. Present results

- Display each URL as a clickable link or inline image.
- For multiple images, list them with index labels (1/3, 2/3, …).
- Offer to download, save to a project, or run another variation.

## Error handling

| Exit condition | Meaning | Action |
|---|---|---|
| `LETMEGO_API_KEY` missing | Token not set | Guide user to picnow settings |
| HTTP 401 | Token invalid | Ask user to check or refresh token |
| HTTP 402 / quota | Insufficient balance | Direct to letmego.top recharge |
| HTTP 4xx other | Bad request | Show upstream error message |
| Network error | Connection issue | Suggest retry |

## Examples

**Text-to-image:**
```
User: 帮我画一张珠宝产品主图，白色背景，高清
→ quality=2k, aspect=square
→ node "$SKILL_DIR/scripts/generate.js" \
    --prompt "jewelry product hero shot, white background, studio lighting, photorealistic" \
    --quality 2k --aspect square --n 1
```

**Image-to-image:**
```
User: 把这张图改成动漫风格 [uploads reference.jpg]
→ quality=1k, ref=reference.jpg
→ node "$SKILL_DIR/scripts/generate.js" \
    --prompt "anime style illustration, vibrant colors, clean linework" \
    --quality 1k --aspect square --n 1 --ref reference.jpg
```

**Landscape banner:**
```
User: Generate a landscape banner for a coffee brand
→ quality=1k, aspect=landscape, n=2
→ node "$SKILL_DIR/scripts/generate.js" \
    --prompt "coffee brand lifestyle banner, warm tones, morning light, artisan aesthetic" \
    --quality 1k --aspect landscape --n 2
```
