# Personal Skills Pack

A curated set of AI agent skills synced via skillshare. Combines skills I've authored with selected skills cherry-picked from third-party "upstream" repos.

## Language

**Pack**:
The curation set as a whole — authored skills plus selected upstream skills, treated as one installable unit. This repo *is* a pack.
_Avoid_: Library, collection, bundle.

**Authored skill**:
A skill I wrote, living under this repo's `skills/` directory. Versioned by this repo's git history.
_Avoid_: Local skill, own skill.

**Upstream**:
A third-party repo I source skills from (e.g., `mattpocock/skills`, `PaddleHQ/agent-skills`). Treated read-only; I never edit upstream content.
_Avoid_: Source, dependency, vendor.

**Curated upstream skill**:
A specific skill from an upstream that I've explicitly picked into the pack via `skills.yaml`. Not all of an upstream's skills end up in my pack — selection is intentional.
_Avoid_: External skill, imported skill.

**Manifest**:
`skills.yaml` at the repo root. Declarative single source of truth for what the pack contains. Editing the manifest is how I add, remove, or re-pin entries.
_Avoid_: Config, recipe, lockfile.

**Bootstrap**:
The `bootstrap` script at the repo root. Translates `skills.yaml` into `skillshare install --track …` invocations. Pure translator; holds no state of its own.
_Avoid_: Installer, runner.

**Sync source**:
The directory skillshare manages on-disk (e.g., `~/.config/skillshare/skills/` or `~/.claude-personal/skills/`), which it then syncs into each AI tool's expected location. The pack ends up here on a given machine; the manifest declares what should be there.
_Avoid_: Install dir, target.

**Float on main**:
The update strategy: upstreams are tracked at their default branch, not pinned to commits. Updates land when I run `skillshare update --all`, not automatically. The opposite of pinning to a SHA.

**Dev mode**:
Bootstrap behaviour when invoked with `--dev`. Authored skills are installed from the local checkout instead of from this repo's git remote. Used while actively editing my own skills; not the default.

## Example dialogue

> "I added a new skill to the pack — `tdd` from `mattpocock/skills`."
> "Authored or upstream?"
> "Upstream. I added a line to the manifest, ran bootstrap, and it landed in the sync source. Skillshare stored its source metadata, so when I run `skillshare update --all` later it'll reinstall the latest from `main`."
> "And the one you've been editing today?"
> "That's authored — `feature-workflow`. I ran bootstrap in dev mode so my edits in the checkout show up live without a re-install."
