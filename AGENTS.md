# AGENTS.md

This is the [claude-code-video-toolkit](https://github.com/digitalsamba/claude-code-video-toolkit) — an AI-native video production workspace. It's built around [Claude Code](https://claude.ai/code) and `CLAUDE.md`, with experimental bridges to other agent runtimes.

For full toolkit guidance — workflow, conventions, timing rules, design patterns, tool catalog — read **[CLAUDE.md](./CLAUDE.md)**. Everything in there applies equally to a Codex (or other-agent) session.

## Codex setup

For local MoviePy + FFmpeg work in OAN, use the focused profile:

```powershell
python -X utf8 scripts/migrate_to_codex.py --map-file codex/local-video-migration.json --no-sync-agents
```

It installs `ds-moviepy`, `ds-ffmpeg`, and `video-toolkit`, with absolute resource
paths. See [docs/codex.md](./docs/codex.md) for Windows fonts, system FFmpeg and
refresh instructions. Read and write all Chinese text files explicitly as UTF-8.

If you're using this repo from Codex, run the migration script once to install the toolkit's skills under `~/.codex/skills/`:

```bash
uv run scripts/migrate_to_codex.py --force
```

The set is discovered dynamically from toolkit skills and command entries. The script can also sync the full `CLAUDE.md` content into a generated block at the end of this file — re-run `--force` after editing `CLAUDE.md` to keep the synced block fresh. Manual content above the generated block (i.e. everything you're reading now) is preserved.

To uninstall:

```bash
uv run scripts/migrate_to_codex.py --reset
```

See [README.md § Using with Codex](./README.md#using-with-codex) for details.
