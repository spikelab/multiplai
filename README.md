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
- **~35 skills in 7 plugin packs.** Autonomous spec-driven builds, deep
  research, transcription, Slack and Gmail, PM artefacts, long-form writing.
- **A native cockpit.** A macOS/iOS app to observe, drive, and triage many
  sessions at once — a session is a file, so watching is free.

## Five repos, one product

| Repo | Role |
|---|---|
| [multiplai-container](https://github.com/spikelab/multiplai-container) | The sandbox — Docker image + macOS SSH bridge. Usable standalone. |
| [multiplai-cc-mktplace](https://github.com/spikelab/multiplai-cc-mktplace) | The features — plugin marketplace: memory engine + six skill packs. Works on vanilla Claude Code. |
| [multiplai-kit](https://github.com/spikelab/multiplai-kit) | Distribution — `setup.sh` + `claude.sh`; the full environment, without touching your `~/.claude`. |
| [multiplai-core](https://github.com/spikelab/multiplai-core) | Shared library — the typed Python plumbing the plugin scripts pin. |
| [multiplai-gui](https://github.com/spikelab/multiplai-gui) | The cockpit — FastAPI hub + SwiftUI app for session orchestration. |

## Which part do you need?

Start small — each row builds on the one above it.

| You want | Get | Requires |
|---|---|---|
| Safe YOLO mode for the Claude Code you already have | [multiplai-container](https://github.com/spikelab/multiplai-container) standalone: clone, `./build.sh`, `docker run` | Docker |
| Memory + skills on your existing Claude Code | [multiplai-cc-mktplace](https://github.com/spikelab/multiplai-cc-mktplace): `claude plugin marketplace add spikelab/multiplai-cc-mktplace` | `uv` |
| The full environment — sandbox, plugins, workspace, memory | [multiplai-kit](https://github.com/spikelab/multiplai-kit): clone, `./setup.sh`, `./claude.sh` | Docker/OrbStack; macOS for bridge skills |
| Many sessions, one cockpit | [multiplai-gui](https://github.com/spikelab/multiplai-gui): hub + app on top of the kit | macOS |

## How it all fits together

See [ARCHITECTURE.md](./ARCHITECTURE.md) — the component map, the interlock
diagram, and the delivery contracts (everything ships as immutable tags;
merging alone delivers nothing).

## Docs

The full docs site is being built from this repo (mkdocs, deployed via GitHub
Pages). Until it's live, each repo's README is the deepest source for its own
component, and ARCHITECTURE.md is the map.

## Status

Young, personal, built in public. Expect fast movement on `main` everywhere;
what's released is what's tagged.
