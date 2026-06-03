# skills

My personal skillshare **source**: the skills I've authored plus the record of which upstream repos I track. This repo *is* the directory skillshare syncs from, so its git history is my cross-machine sync.

See [`CONTEXT.md`](./CONTEXT.md) for vocabulary and [`docs/adr/`](./docs/adr/) for the load-bearing decisions.

## How it works

- skillshare's **source** is pointed at this repo. It holds my **authored skills** and skillshare's own bookkeeping.
- I **track whole upstream repos** rather than cherry-picking skills. skillshare records each in `.metadata.json` and re-clones them on every machine; the clones are gitignored, not vendored in.
- I switch individual skills on/off in skillshare's TUI. That toggle state lives in `.skillignore`.
- `.metadata.json` (tracked-repo list) and `.skillignore` (toggles) are committed here, so `skillshare push` / `pull` carries my whole setup between machines. `config.yaml` is machine-local (source path + tool dirs) and deliberately stays *out* of this repo.

## Layout

```
skills/              # authored skills (one dir per skill, each with SKILL.md)
.metadata.json       # skillshare: which upstream repos are tracked   (synced)
.skillignore         # skillshare: which skills are enabled/disabled  (synced)
.gitignore           # skillshare manages a block here ignoring the cloned upstream dirs
docs/adr/            # architecture decision records
CONTEXT.md           # glossary of project-specific terms
```

## Tracked upstreams

- `PaddleHQ/agent-skills` (main)
- `mattpocock/skills` (main)
- `ibelick/ui-skills` (main)

`.metadata.json` is the authoritative list; the above is the human-readable mirror.

## Setup on a new machine

```bash
# install skillshare per its own instructions, then:
git clone <this-repo> ~/projects/skills
skillshare init --source ~/projects/skills --all-targets   # point skillshare at this repo (see `skillshare init --help`)
skillshare install                                         # re-clone tracked upstreams from .metadata.json
skillshare sync                                            # fan out to your AI tools
```

## Day to day

- **Add an authored skill** — create `skills/<name>/SKILL.md` → `skillshare sync`.
- **Track a new upstream** — `skillshare install <user>/<repo> --track` → `skillshare sync`. (Also add it to the list above.)
- **Toggle a skill** — skillshare's TUI (press `E` in `skillshare list`) or `skillshare disable <name>` → `skillshare sync`.
- **Update upstreams** — `skillshare update --all` → `skillshare sync` (float-on-main; nothing's pinned).
- **Sync across machines** — `skillshare push` here, `skillshare pull` on the other machine.
