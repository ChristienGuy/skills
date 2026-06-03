# skillshare as the engine, with this repo as its source

We use skillshare to install skills, track upstream repos, and sync them to every AI tool — and we point skillshare's **source** directory at *this repo* instead of its default `~/.config/skillshare/skills`. That makes this repo the version-controlled home for my authored skills and for skillshare's tracked-repo record (`.metadata.json`) and toggle state (`.skillignore`), so `skillshare push`/`pull` *is* my cross-machine sync — no custom script needed.

We chose skillshare over Claude Code's native plugin marketplace (Claude-only; each entry is a plugin, not a skill) and over a custom clone-and-symlink tool (skillshare already does multi-tool sync, audit gating, trash/restore, and TUI toggling — re-implementing that buys nothing).

Consequence worth flagging: skillshare actively writes into this repo. It clones tracked upstreams in (gitignored, not vendored) and manages `.metadata.json`, `.skillignore`, and a block in `.gitignore`. A reader surprised to find `PaddleHQ/agent-skills` cloned inside a "projects" repo should know it's deliberate — this repo *is* skillshare's source. Note that `config.yaml` (source path + target dirs) lives outside the source and is intentionally machine-local, so it does not sync.
