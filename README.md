# skills

Personal skills pack. Cherry-picks specific skills from upstream repos via skillshare and adds my own authored ones.

See [`CONTEXT.md`](./CONTEXT.md) for vocabulary, [`docs/adr/`](./docs/adr/) for the load-bearing design decisions.

## Layout

```
skills.yaml          # what the pack contains (declarative, single source of truth)
bootstrap            # translates skills.yaml -> skillshare install calls
skills/              # authored skills (one dir per skill, each with SKILL.md)
docs/adr/            # architecture decision records
CONTEXT.md           # glossary of project-specific terms
```

## Setup on a new machine

```bash
# Prereqs
brew install bash yq          # macOS ships bash 3.2; bootstrap needs 4+
# install skillshare per its own instructions

# Clone and run
git clone <this-repo>
cd skills
./bootstrap          # installs everything from skills.yaml
skillshare sync      # push to Claude / Cursor / etc.
```

## Adding a skill

1. **Authored**: create `skills/<name>/SKILL.md` and add `<name>` to the `authored:` list in `skills.yaml`.
2. **Upstream**: add an entry under `upstreams:` with the repo, ref, and chosen skill names.
3. Run `./bootstrap` to install (`--dev` to install authored skills from the local checkout for live editing).
4. Run `skillshare sync` to push to your AI tools.

## Updating upstreams

Float-on-main: upstreams aren't pinned to commits. Run `skillshare update --all` when you want to pull the latest.
