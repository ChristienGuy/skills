# Declarative manifest, not shell-only bootstrap

The pack's curated selections live in `skills.yaml` (declarative YAML), not directly as `skillshare install` lines inside a shell script. `bootstrap` is a thin translator from the YAML into skillshare invocations.

The simpler alternative — putting `skillshare install -s … --track` lines directly in a `bootstrap.sh` — was rejected for two reasons. First, a future local TUI or GUI for browsing and toggling skills can read and write `skills.yaml` directly; it cannot safely round-trip changes through arbitrary shell. Second, the manifest produces a clean diff per change: `git log skills.yaml` reads as a curation history (when was X added, when was Y dropped, from which upstream). Shell bootstrap mixes intent and implementation on the same lines and reads worse over time.

Cost: ~30 extra lines of bash to parse YAML with `yq` and loop over it. Negligible.
