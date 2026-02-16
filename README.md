# Codex Skills (Organization)

This repository contains organization-approved Codex skills.

## Repository Layout

- `skills/` - one folder per skill
- `skills/<skill-name>/SKILL.md` - required skill definition
- Optional inside each skill: `scripts/`, `references/`, `assets/`

## Included Skills

- `confluence-to-stories`

## Install (single skill)

```bash
python ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo your-org/codex-skills \
  --path skills/confluence-to-stories
```

## Install (specific version)

```bash
python ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo your-org/codex-skills \
  --ref v1.0.0 \
  --path skills/confluence-to-stories
```

After installation, restart Codex to load new skills.
