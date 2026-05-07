# picnow — Image Generation Skill

Generate high-quality images from any SKILL.md-compatible AI agent.  
Text-to-image · Image-to-image · 1K / 2K / 4K · GPT-Image-2

**Install page:** https://picnow.letmego.top/skills

---

## Install

```bash
# Claude Code (recommended)
npx skillpm install picnow

# SkillHub
skillhub install picnow

# ClawHub
clawdhub install picnow

# Manual (Claude Code)
git clone https://github.com/jeekchen/picnow-skill
cp -r picnow-skill ~/.claude/skills/picnow
```

## Set up your token (令牌)

Get your token at **https://picnow.letmego.top/settings**, then:

```bash
export LETMEGO_API_KEY=your_令牌
```

Add this to your shell profile (`~/.zshrc` or `~/.bashrc`) to persist it.

## Usage

Once installed and token set, just talk to your agent:

```
> 帮我画一张珠宝产品主图，白色背景，高清
> Generate a landscape banner for a coffee brand
> 把这张图改成动漫风格  [attach reference.jpg]
```

The skill activates automatically when you ask to create or edit images.

## Direct script usage

```bash
# Text-to-image
LETMEGO_API_KEY=your_token node scripts/generate.js \
  --prompt "product photo, white background, studio lighting" \
  --quality 2k \
  --aspect square \
  --n 1

# Image-to-image (edit)
LETMEGO_API_KEY=your_token node scripts/generate.js \
  --prompt "anime style, vibrant colors" \
  --quality 1k \
  --aspect square \
  --ref ./reference.jpg
```

Output: `{ "urls": ["https://..."] }`

## Quality tiers

| Quality | Resolution | Use case |
|---------|-----------|----------|
| `1k` | up to 1536 × 1536 | Preview, social media |
| `2k` | up to 2048 × 2048 | E-commerce, print |
| `4k` | up to 3840 × 2160 | Landscape / portrait only |

## Supported clients

- Claude Code
- OpenAI Codex CLI
- OpenClaw
- Any agent supporting SKILL.md

## Published on

[![SkillsMP](https://img.shields.io/badge/SkillsMP-listed-blue)](https://skillsmp.com)
[![ClawHub](https://img.shields.io/badge/ClawHub-listed-purple)](https://clawhub.ai)
[![SkillHub](https://img.shields.io/badge/SkillHub-listed-green)](https://skillhub.club)

## License

MIT
