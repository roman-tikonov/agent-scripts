# AGENTS.MD

Start: say hi + 1 motivating line.
Work style: telegraph; noun-phrases ok; drop grammar; min tokens.

## Agent Protocol

- Workspace: `~/Projects`.
- Files: repo or `~/Projects/agent-scripts`.
- PRs: use `gh pr view/diff` (no URLs).
- “Make a note” => edit `~/Projects/agent-scripts/AGENTS.md` (shortcut; not a blocker). Ignore `CLAUDE.md`.
- Notes location: store notes in the active project folder (for example `<project>/notes/` or project-local docs notes).
- Notes index: keep any note index inside the active project repository (for example `<project>/notes/notes_index.md`); do not maintain a shared global notes index file.
- Every launch: read the active project's local note index if present, then read relevant linked project notes before implementation.
- Complex tasks: write/update a short note in the active project folder during or right after implementation; then add/update that project's local note index; goal: faster future runs, less re-discovery.
- No `./runner`. Guardrails: use `trash` for deletes.
- Store temporary files under a `.tmp/` folder in the relevant project.
- Need upstream file: stage in `/tmp/`, then cherry-pick; never overwrite tracked.
- Bugs: add regression test when it fits.
- Keep files <~500 LOC; split/refactor as needed.
- Commits: Conventional Commits (`feat|fix|refactor|build|ci|chore|docs|style|perf|test`).
- Subagents: read `docs/subagent.md`.
- Editor: `code <path>`.
- CI: `gh run list/view` (rerun/fix til green).
- Prefer end-to-end verify; if blocked, say what’s missing.
- New deps: before installing any project dependency, verify it appears safe and non-malicious. Look it up online using trustworthy sources (package registry, source repo, release history, issue tracker, security advisories, maintainer reputation). Warn the user before install if suspicious.
- Dependency safety criteria: expected package name/scope/repo/maintainer; no typosquatting or dependency-confusion signs; reasonable adoption/downloads; recent legitimate releases/commits; no unresolved critical advisories or malware reports; no suspicious install/postinstall scripts; no unusual obfuscated code, unexpected binaries, credential access, or broad filesystem/network behavior; registry tarball matches repo/license/published files; dependency is actually necessary versus existing project deps or standard-library/native alternatives.
- Slash cmds: `~/.codex/prompts/`.
- Skills: never install skills without explicit user permission first.
- Skills: always install into `~/.agents/skills` (never `~/.codex/skills`).
- Web: search early; quote exact errors; prefer 2025–2026 sources; fallback Firecrawl (`pnpm mcp:*`) / `mcporter`.
- Style: telegraph. Drop filler/grammar. Min tokens (global AGENTS + replies).

## Coding and system design conventions
- Coding principles: separation of concerns, modularisation.  
- Prefer static-typed data models over free-form dictionaries for payloads and structured state.
- Always choose the strictest type applicable when implementing a parameter, response type, or variable.
- Keep module public surfaces minimal; make helpers/constants private unless used outside the module.
- Avoid chain imports and re-export coupling; import from the owning module unless a stable public module is intentionally defined and maintained.
- If the same API endpoint is called from two different modules in a project, extract that endpoint call into a separate reusable module.


## Screenshots (“use a screenshot”)
- Pick newest PNG in `~/Desktop` or `~/Downloads`.
- Verify it’s the right UI (ignore filename).
- Size: `sips -g pixelWidth -g pixelHeight <file>` (prefer 2×).
- Optimize: `imageoptim <file>` (install: `brew install imageoptim-cli`).
- Replace asset; keep dimensions; commit; run gate; verify CI.

## Important Locations
- Project-local notes index: for example `<project>/notes/notes_index.md`
- Task implementation notes: project-local note files

## Docs
- Start: run docs list (`docs:list` script, or `bin/docs-list` here if present; ignore if not installed); open docs before coding.
- Follow links until domain makes sense; honor `Read when` hints.
- Keep notes short; update docs when behavior/API changes (no ship w/o docs).
- Add `read_when` hints on cross-cutting docs.
- Model preference: latest only. OK: Anthropic Opus 4.5 / Sonnet 4.5 (Sonnet 3.5 = old; avoid), OpenAI GPT-5.2, xAI Grok-4.1 Fast, Google Gemini 3 Flash.

## PR Feedback
- Active PR: `gh pr view --json number,title,url --jq '"PR #\\(.number): \\(.title)\\n\\(.url)"'`.
- PR comments: `gh pr view …` + `gh api …/comments --paginate`.
- Replies: cite fix + file/line; resolve threads only after fix lands.
- When merging a PR: thank the contributor in `CHANGELOG.md`.

## Flow & Runtime
- Use repo’s package manager/runtime; no swaps w/o approval.
- Use Codex background for long jobs; tmux only for interactive/persistent (debugger/server).

## Post-Implementation Checklist (Required after big changes in existing files)
- Review error messages for correctness and tighten them (actionable, specific, no ambiguity).
- For CLI tools, review and update CLI help text/command descriptions for changed behavior.
- Check for and remove unused code introduced during implementation.

## Build / Test
- Before handoff: run full gate (lint/typecheck/tests/docs).
- CI red: `gh run list/view`, rerun, fix, push, repeat til green.
- Keep it observable (logs, panes, tails, MCP/browser tools).
- Reminder: check `~/.profile` and `~/.bashrc` for missing env keys.

## Git
- Safe by default: `git status/diff/log`. Push only when user asks.
- `git checkout` ok for PR review / explicit request.
- Branch changes require user consent.
- Destructive ops forbidden unless explicit (`reset --hard`, `clean`, `restore`, `rm`, …).
- Remotes under `~/Projects`: prefer HTTPS; flip SSH->HTTPS before pull/push.
- Don’t delete/rename unexpected stuff; stop + ask.
- No repo-wide S/R scripts; keep edits small/reviewable.
- Avoid manual `git stash`; if Git auto-stashes during pull/rebase, that’s fine (hint, not hard guardrail).
- If user types a command (“pull and push”), that’s consent for that command.
- No amend unless asked.
- Big review: `git --no-pager diff --color=never`.
- Multi-agent: check `git status/diff` before edits; ship small commits.

## Language/Stack Notes
- TypeScript: use repo PM; run `docs:list`; keep files small; follow existing patterns.

## Critical Thinking
- Fix root cause (not band-aid).
- Unsure: read more code; if still stuck, ask w/ short options.
- Conflicts: call out; pick safer path.
- Unrecognized changes: assume other agent; keep going; focus your changes. If it causes issues, stop + ask user.
- Leave breadcrumb notes in thread.

## Tools

Read `~/Projects/agent-scripts/tools.md` for the full tool catalog if it exists.

### committer
- Commit helper (PATH). Stages only listed paths; required here. Repo may also ship `./scripts/committer`.

### trash
- Move files to Trash: `trash …` (system command).

### bin/docs-list / scripts/docs-list.ts
- Optional. Lists `docs/` + enforces front-matter. Ignore if `bin/docs-list` not installed. Rebuild: `bun build scripts/docs-list.ts --compile --outfile bin/docs-list`.

### bin/browser-tools / scripts/browser-tools.ts
- Chrome DevTools helper. Cmds: `start`, `nav`, `eval`, `screenshot`, `pick`, `cookies`, `inspect`, `kill`.
- Rebuild: `bun build scripts/browser-tools.ts --compile --target bun --outfile bin/browser-tools`.

### mcporter / iterm / firecrawl / XcodeBuildMCP
- MCP launcher: `npx mcporter <server>` (see `npx mcporter --help`). Common: `iterm`, `firecrawl`, `XcodeBuildMCP`.

### gh
- GitHub CLI for PRs/CI/releases. Given issue/PR URL (or `/pull/5`): use `gh`, not web search.
- Examples: `gh issue view <url> --comments -R owner/repo`, `gh pr view <url> --comments --files -R owner/repo`.

### Slash Commands
- Global: `~/.codex/prompts/`. Repo-local: `docs/slash-commands/`.
- Common: `/handoff`, `/pickup`.

### tmux
- Use only when you need persistence/interaction (debugger/server).
- Quick refs: `tmux new -d -s codex-shell`, `tmux attach -t codex-shell`, `tmux list-sessions`, `tmux kill-session -t codex-shell`.

<frontend_aesthetics>
Avoid “AI slop” UI. Be opinionated + distinctive.

Do:
- Typography: pick a real font; avoid Inter/Roboto/Arial/system defaults.
- Theme: commit to a palette; use CSS vars; bold accents > timid gradients.
- Motion: 1–2 high-impact moments (staggered reveal beats random micro-anim).
- Background: add depth (gradients/patterns), not flat default.

Avoid: purple-on-white clichés, generic component grids, predictable layouts.
</frontend_aesthetics>
