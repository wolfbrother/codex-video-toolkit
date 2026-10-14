# Using with Codex

## OAN / Windows local video profile

The fork supports a focused profile for local MoviePy + FFmpeg work:

```powershell
python -X utf8 scripts/migrate_to_codex.py --map-file codex/local-video-migration.json --no-sync-agents
```

Run this from the toolkit root. It installs `ds-moviepy`, `ds-ffmpeg`, and
`video-toolkit`, leaving cloud-generation and publishing workflows uninstalled.
Destination defaults to `$CODEX_HOME/skills` or `~/.codex/skills`; override with
`--dest-root`. Use `--dry-run` to inspect, `--force` to refresh these installed
skills, and the same map and destination with `--reset` to remove them.

Installed skills include the absolute toolkit root so they work from the parent
OAN workspace. Moving the checkout requires reinstalling. Original Claude assets
are preserved. `--no-sync-agents` avoids duplicating the full CLAUDE.md because
the existing AGENTS.md already links it. Reopen the Codex chat to refresh skill
discovery; installation cannot change an active turn's skill catalog.

For existing local dependencies, use the installed Python directly for MoviePy
examples. The broader toolkit recommends `uv sync`, which creates its own venv
and includes optional-service client libraries; it is not necessary for a basic
MoviePy render when its imports are already available.

```powershell
$env:FFMPEG_BINARY = (Get-Command ffmpeg.exe).Source
python -X utf8 examples/quick-spot/build.py
```

Set `FFMPEG_BINARY` before Python starts: MoviePy can otherwise select the
imageio-provided binary instead of system FFmpeg. For Chinese overlays select
`C:/Windows/Fonts/msyh.ttc` or another installed CJK font, and read/write text
files explicitly as UTF-8. Bash commands and macOS font paths in upstream
guides need conversion on Windows. Browser tasks follow the current Codex
browser skill, rather than taking over a personal browser profile.

The generated wrappers are Skill entrypoints, not native Claude slash commands.
Full migration is still available below; it includes cloud services and
publishing workflows with their own setup requirements.

This toolkit is built around Claude Code assets in `.claude/` and `CLAUDE.md`, but it
also ships an experimental migration script for [Codex](https://openai.com/codex/) —
contributed by [@kimhoontae-gogo](https://github.com/kimhoontae-gogo) in
[#16](https://github.com/digitalsamba/claude-code-video-toolkit/pull/16).

```bash
uv run scripts/migrate_to_codex.py --force
```

This does two things:

1. **Installs Codex skills into `~/.codex/skills`** — one skill per directory found in
   `.claude/skills/` (domain knowledge: remotion, ltx2, ideogram4, acestep, …) plus the
   guided workflows from `.claude/commands/` (video, brand, voice-clone, …). The set is
   discovered dynamically, so newly added toolkit skills are picked up on the next run;
   the script prints exactly what it installed.
2. **Generates a Codex block in the repository root `AGENTS.md`**, derived from
   `CLAUDE.md`.

## The AGENTS.md block

- The script manages **only** its generated block inside `AGENTS.md`.
- Manual `AGENTS.md` content outside that block is preserved.
- The block is derived from `CLAUDE.md` — after `CLAUDE.md` changes, re-run
  `uv run scripts/migrate_to_codex.py --force` to refresh it.

## Removing

```bash
uv run scripts/migrate_to_codex.py --reset
```

`--reset` removes the toolkit skills previously installed under `~/.codex/skills` and
removes the generated Codex block from `AGENTS.md`. It does not delete other user
skills and it does not remove the rest of `AGENTS.md`.
