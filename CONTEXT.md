# Personal Skills Source

This repo is my skillshare **source**: the directory skillshare manages and syncs to every AI tool I use. It holds the skills I've authored plus the record of which third-party repos I track, so skillshare's own `push`/`pull` version-controls and carries my setup between machines.

## Language

**Source**:
The single directory skillshare owns and syncs *from* — this repo *is* it. skillshare writes here: it records tracked repos, clones them in, and manages toggle state.
_Avoid_: Pack, library, install dir.

**Target**:
A specific AI tool's skills directory that skillshare syncs *into* (Claude, Cursor, Codex, …). One source fans out to many targets.
_Avoid_: Tool, destination, output.

**Authored skill**:
A skill I wrote, living in this source and versioned by this repo's git history. Has no upstream.
_Avoid_: Local skill, own skill, my skill.

**Tracked repo** (a.k.a. **upstream**):
A whole third-party skills repo I follow as a unit (e.g. `PaddleHQ/agent-skills`, `mattpocock/skills`). skillshare clones it in and re-clones it on each machine; I never edit its contents.
_Avoid_: Dependency, vendor, cherry-pick.

**Track**:
The act of adding a tracked repo to the source as a unit. The opposite of cherry-picking individual skills out of it.
_Avoid_: Install, import, pin.

**Toggle**:
Enabling or disabling one skill within a tracked repo. This on/off switch is the only per-skill curation I keep — I track repos whole and toggle off what I don't want.
_Avoid_: Allowlist, selection, enable/disable list.

**Synced state**:
The two things that travel between machines: the tracked-repo record and the toggle state. Both live inside this source and ride skillshare's `push`/`pull`.
_Avoid_: Config, manifest, lockfile.

**Machine-local config**:
skillshare settings that belong to one machine — the source path and the set of target directories. Deliberately *not* synced; lives outside this repo.
_Avoid_: Config (unqualified), settings.

**Float on main**:
The update strategy: tracked repos follow their default branch, never a pinned commit. Updates land only when I run a skillshare update.
_Avoid_: Pin, lock to SHA.

## Example dialogue

> "I want `tdd` from mattpocock but not the rest of his repo."
> "You still track the repo whole — `mattpocock/skills` is one tracked repo. You just toggle off the skills you don't want in skillshare's TUI. That on/off switch is the only per-skill thing we keep."
> "And where does that switch live?"
> "In the synced state, next to the tracked-repo record — both sit in this source, so a `pull` on the laptop reproduces the exact same set: it re-clones the upstreams and re-applies the same toggles."
> "What about a skill I write myself?"
> "That's an authored skill — it lives here and is versioned by this repo. No upstream, nothing to track."
