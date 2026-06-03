# Skillshare as the install/update engine

We considered three mechanisms for installing and updating skills in this pack: Claude Code's native plugin marketplace (`.claude-plugin/marketplace.json`), a custom clone-and-symlink tool, and skillshare. We chose **skillshare** because (1) it already supports cherry-picking individual skills from multi-skill upstreams via `-s`/`--track`, (2) it syncs to multiple AI tools (Claude today, Cursor / Windsurf / Codex / etc. later) so the pack stays portable if I switch tools, and (3) it's mature — audit gating, `.skillignore`, hubs, trash/restore — none of which we'd want to re-implement.

The native marketplace was rejected because each entry is a *plugin*, not a *skill*; cherry-picking individual skills out of multi-skill upstreams forces `git-subdir` entries that work but feel off-label, and it's Claude-only. A custom tool was rejected because skillshare already does the job and replacing it would mean owning a tool whose value-over-skillshare is small.

Reversing this would mean re-importing every upstream selection into a new engine and rewriting `bootstrap`. The contract `skills.yaml` declares is engine-agnostic; the lock-in is mostly in `bootstrap`.
