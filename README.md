# Multiplai

> Claude Code, with a memory and a safety net.

Every Claude Code session starts from zero. It doesn't remember what you built
yesterday, the permission prompts interrupt every third command — or you switch
them off and hope for the best. Multiplai fixes both, then keeps going.

**What you get:**

- **A memory that compounds.** Relevant context is routed into every session
  automatically. Sessions keep a diary, capture learnings, and consolidate them
  into long-term memory — through a review step ("dreams") where you approve
  every change before it's written.
- **A sandbox that makes YOLO mode safe.** Sessions run
  `--dangerously-skip-permissions` inside a Docker container that *is* the
  permission boundary, with a key-restricted SSH bridge for the few tools that
  genuinely need your Mac (Xcode, Whisper, your real Chrome).
- **40+ skills — the memory engine plus six skill packs.** Autonomous
  spec-driven builds, deep research, transcription, Slack and Gmail, PM
  artefacts, long-form writing. The authoritative list is the compatibility
  matrix in the
  [multiplai-cc-mktplace README](https://github.com/spikelab/multiplai-cc-mktplace#compatibility-matrix).

On the roadmap, not yet released: **multiplai-gui**, a native macOS/iOS cockpit
for observing and driving many sessions at once.

## Start here

Add the plugin marketplace from inside Claude Code:

```
/plugin marketplace add spikelab/multiplai-cc-mktplace
```

Then `/plugin install multiplai-context@multiplai` for the memory engine. Want
the sandboxed full environment instead? Clone
[multiplai-kit](https://github.com/spikelab/multiplai-kit) and run `./setup.sh`.

## The repos

| Repo | Role |
|---|---|
| [multiplai-container](https://github.com/spikelab/multiplai-container) | The sandbox — Docker image + macOS SSH bridge. Usable standalone. |
| [multiplai-cc-mktplace](https://github.com/spikelab/multiplai-cc-mktplace) | The features — plugin marketplace: memory engine + six skill packs. Works on vanilla Claude Code. |
| [multiplai-kit](https://github.com/spikelab/multiplai-kit) | Distribution — `setup.sh` + `claude.sh`; the full environment, without touching your `~/.claude`. |
| [multiplai-core](https://github.com/spikelab/multiplai-core) | Shared library — the typed Python plumbing the plugin scripts pin. |
| **multiplai-gui** *(coming soon)* | The cockpit — a FastAPI hub + SwiftUI app for session orchestration. Not yet released; the repo is private until it is. |

## Which part do you need?

Start small — each row builds on the one above it.

| You want | Get | Requires |
|---|---|---|
| Safe YOLO mode for the Claude Code you already have | [multiplai-container](https://github.com/spikelab/multiplai-container) standalone: clone, `./build.sh`, `docker run` | Docker |
| Memory + skills on your existing Claude Code | [multiplai-cc-mktplace](https://github.com/spikelab/multiplai-cc-mktplace): `claude plugin marketplace add spikelab/multiplai-cc-mktplace` | `uv` |
| The full environment — sandbox, plugins, workspace, memory | [multiplai-kit](https://github.com/spikelab/multiplai-kit): clone, `./setup.sh`, `./claude.sh` | Docker/OrbStack; macOS for bridge skills |
| Many sessions, one cockpit | **multiplai-gui** — hub + app on top of the kit. *Not yet released.* | macOS |

## How it all fits together

See [ARCHITECTURE.md](./ARCHITECTURE.md) — the component map, the interlock
diagram, and the delivery contracts (everything ships as immutable tags;
merging alone delivers nothing).

## Docs

The docs are the READMEs. Each repo's README is the deepest source for its own
component; [ARCHITECTURE.md](./ARCHITECTURE.md) is the map across them; the
changelogs below are the release record. A dedicated docs site may follow.

- [multiplai-container README](https://github.com/spikelab/multiplai-container#readme) — image contents, the SSH bridge, tag/release contract
- [multiplai-cc-mktplace README](https://github.com/spikelab/multiplai-cc-mktplace#readme) — every plugin and skill, the compatibility matrix, what runs unattended
- [multiplai-kit README](https://github.com/spikelab/multiplai-kit#readme) — install, configuration, credentials blast radius
- [multiplai-core README](https://github.com/spikelab/multiplai-core#readme) — the library API and its pinning contract
- [CHANGELOG.md](./CHANGELOG.md) — changes to this repo's documentation

### Releases

What shipped recently, per component:

<!-- TODO(changelog-link): component CHANGELOG.md files are being added by the
     sibling repo plans and are not on their `main` branches yet. When they land,
     repoint these at the files themselves, e.g.
     https://github.com/spikelab/multiplai-core/blob/main/CHANGELOG.md -->

- [multiplai-container releases](https://github.com/spikelab/multiplai-container/releases) — the tag consumed by the kit's `CONTAINER_REF` pin
- [multiplai-core releases](https://github.com/spikelab/multiplai-core/releases) — the tags plugin scripts pin
- [multiplai-cc-mktplace releases](https://github.com/spikelab/multiplai-cc-mktplace/releases) — per-plugin `<plugin>@<version>` tags
- [multiplai-kit releases](https://github.com/spikelab/multiplai-kit/releases) — untagged so far; the kit is consumed by `git pull && ./setup.sh`

## Status

Young, personal, built in public. Expect fast movement on `main` everywhere;
what's released is what's tagged.
