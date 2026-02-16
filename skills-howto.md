# Skill How-To (Codex)

## Discover

Curated skills:

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/list-skills.py
```

Filter by keyword:

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/list-skills.py | rg -i "vercel|netlify"
```

Experimental skills:

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/list-skills.py --path skills/.experimental
```

## Install

Install curated skill(s) to `~/.agents/skills`:

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo openai/skills \
  --path skills/.curated/<skill-name> [skills/.curated/<another-skill>] \
  --dest ~/.agents/skills
```

Install from any GitHub URL/path:

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --url https://github.com/<owner>/<repo>/tree/<ref>/<path-to-skill> \
  --dest ~/.agents/skills
```

After install: restart Codex to load new skills.
