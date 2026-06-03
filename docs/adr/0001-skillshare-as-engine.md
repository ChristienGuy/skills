# Skillshare as the install/update engine

We considered three mechanisms for installing and updating skills in this pack: Claude Code's native plugin marketplace (`.claude-plugin/marketplace.json`), a custom clone-and-symlink tool, and skillshare. We chose **skillshare** because (1) it supports cherry-picking individual skills from multi-skill upstreams via `--skill`, (2) it syncs to multiple AI tools (Claude today, Cursor / Windsurf / Codex / etc. later) so the pack stays portable if I switch tools, and (3) it's mature — audit gating, `.skillignore`, hubs, trash/restore — none of which we'd want to re-implement.

The native marketplace was rejected because each entry is a *plugin*, not a *skill*; cherry-picking individual skills out of multi-skill upstreams forces `git-subdir` entries that work but feel off-label, and it's Claude-only. A custom tool was rejected because skillshare already does the job and replacing it would mean owning a tool whose value-over-skillshare is small.

## Implementation notes discovered during bootstrap

- **`--skill` and `--track` are mutually exclusive** in skillshare. `--track` (Team Edition) clones a whole upstream and updates via `git pull`; `--skill` cherry-picks and updates via reinstall-from-stored-source. We use `--skill`. `skillshare update --all` still works — it just reinstalls each cherry-picked skill rather than git-pulling a tracked clone.
- **`--skill` arguments are skill *names***, not paths. A skill at `skills/engineering/tdd/` is referenced as `tdd`, not `skills/engineering/tdd`. This holds even for upstreams that nest skills under category directories.
- **Private HTTPS clones need `GITHUB_TOKEN` in the environment**; skillshare runs git with prompts disabled so credential helpers don't work. `bootstrap` plumbs through `gh auth token` automatically if available.

Reversing this would mean re-importing every upstream selection into a new engine and rewriting `bootstrap`. The contract `skills.yaml` declares is engine-agnostic; the lock-in is mostly in `bootstrap`.
