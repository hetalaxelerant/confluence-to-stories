# Confluence to Stories

This repository tracks skill configuration and generated planning artifacts for the Confluence-to-backlog workflow.

## Current Repository Layout

- `confluence-to-stories/SKILL.md`  
  Skill definition used to convert Confluence requirements into epics/stories/sprint planning output.
- `confluence-to-stories/README.md`  
  Skill-specific notes.
- `output/stories-YYYY-MM-DD.md`  
  Generated/refined user stories.
- `output/sprint-plan-YYYY-MM-DD.md`  
  Generated sprint plans.
- `docs/`  
  Optional supporting documentation.

## Current Skill Behavior

The `confluence-to-stories` skill is configured to:

1. Ask on first prompt whether Jira/Confluence MCP setup should be done now.
2. If setup succeeds, use Jira mode (ask for Jira project key, then create epics/stories in Jira).
3. If setup is skipped or fails, continue in file-only mode (no blocking), writing markdown artifacts.

## Local Setup

```bash
git clone https://github.com/hetalaxelerant/confluence-to-stories.git
cd confluence-to-stories
```

For updates:

```bash
git pull origin main
```

## Publishing Local Changes

```bash
git add -A
git commit -m "Update skills and planning artifacts"
git pull --rebase origin main
git push origin main
```

## Notes

- Keep generated outputs in `output/` instead of repository root.
- Keep temporary/editor files out of git (`.DS_Store`, `*.save` are ignored).

## Troubleshooting

### `fatal: 'origin' does not appear to be a git repository`

```bash
git remote -v
git remote add origin https://github.com/hetalaxelerant/confluence-to-stories.git
```

If `origin` exists but is wrong:

```bash
git remote set-url origin https://github.com/hetalaxelerant/confluence-to-stories.git
```

### `! [rejected] main -> main (fetch first)`

```bash
git fetch origin
git pull --rebase origin main
git push origin main
```

### Rebase conflict (`CONFLICT ...`)

```bash
git status
```

Resolve conflicted file(s), then:

```bash
git add <resolved-file>
GIT_EDITOR=true git rebase --continue
git push origin main
```

### Vim swap file warning (`.COMMIT_EDITMSG.swp`)

```bash
rm -f .git/.COMMIT_EDITMSG.swp
GIT_EDITOR=true git rebase --continue
```

### `zsh: parse error near ')'`

- Run one command per line.
- Do not paste explanatory text/hints from terminal output.
- Do not use placeholders like `<your-branch>` literally in commands.
