# The Multiplai Suite

> Canonical copy. This lives in [`spikelab/multiplai`](https://github.com/spikelab/multiplai),
> the suite's umbrella repo; the five component repos link here instead of carrying
> their own copies.

Multiplai turns [Claude Code](https://docs.anthropic.com/en/docs/claude-code) into a
persistent working environment that compounds — it learns what you approve:

- a **memory + learning loop** (context routing, session diary, learnings, dreams) that
  makes every session smarter than the last;
- a **sandboxed container runtime** that makes `--dangerously-skip-permissions` safe,
  because the container itself is the permission boundary;
- **the memory engine plus six skill packs** — 40+ skills covering development,
  research, media, messaging, product management, and writing (the authoritative list
  is the compatibility matrix in the
  [multiplai-cc-mktplace README](https://github.com/spikelab/multiplai-cc-mktplace#compatibility-matrix));
- and, on the roadmap, a **native macOS/iOS app** for observing and orchestrating many
  sessions at once — not yet released.

Four published repos plus this umbrella, with the cockpit still to come.

## Components

| Repo | Role | One-liner |
|---|---|---|
| [multiplai-container](https://github.com/spikelab/multiplai-container) | The sandbox | Docker image with a pinned toolchain + a key-restricted macOS SSH bridge for host-only tools (Xcode, whisper, real Chrome). Usable standalone. |
| [multiplai-cc-mktplace](https://github.com/spikelab/multiplai-cc-mktplace) | The features | Claude Code plugin marketplace: `multiplai-context` (the memory engine) plus six themed skill packs. Works on vanilla Claude Code. |
| [multiplai-kit](https://github.com/spikelab/multiplai-kit) | Distribution & runtime | What you clone for the full experience: `setup.sh` scaffolds workspace + runtime, `claude.sh` launches sessions; your `~/.claude` stays untouched. |
| [multiplai-core](https://github.com/spikelab/multiplai-core) | Shared library | Typed Python plumbing (paths, config, model client, agent runner, costing, logging) consumed by plugin scripts via immutable git-tag pins. |
| **multiplai-gui** *(coming soon)* | The cockpit | Will be a FastAPI hub + SwiftUI app (macOS/iOS): session board, live feed, chat-driving, dreams triage, costs, memory browser, health. Not yet released; the repo stays private until it is. |

## Which part do I need?

| You want | Get | Requires |
|---|---|---|
| **Memory + skills** on your existing Claude Code | `/plugin marketplace add spikelab/multiplai-cc-mktplace`, install `multiplai-context`, add packs à la carte | `uv` |
| **Safe YOLO mode** — `--dangerously-skip-permissions` for the Claude Code you already have | `multiplai-container` standalone (see its README quickstart) | Docker |
| **The full environment** — sandbox, plugins, workspace, memory, launcher | Clone `multiplai-kit` → `./setup.sh` → `./claude.sh` | Docker/OrbStack; macOS for bridge skills |
| **Many sessions, one cockpit** | `multiplai-gui` hub + app on top of the kit — *not yet released* | macOS host with the kit installed |

Each row wants more of the environment than the one before — plugins → sandbox → kit →
cockpit is an adoption ladder, not four separate products.

## How the repos interlock

```
                    ┌───────── multiplai-gui (hub + app, coming soon) ───────┐
                    │  observes JSONLs, drives sessions, triages dreams      │
                    ▼                                                        │
 user ──► multiplai-kit (claude.sh / setup.sh)                               │
             │  pins tag ──► multiplai-container (image + SSH bridge)        │
             │  installs ──► multiplai-cc-mktplace (7 plugins) ◄─────────────┘
             │                     │  PEP-723 tag pins                (reads .multiplai/,
             ▼                     ▼                                   calls plugin scripts)
        workspace (.multiplai/ memory·diary·learnings·dreams)   multiplai-core (library)
```

## Runtime contract: who knows a session is over

The one piece of shared state the repos write to *at runtime* rather than at
release time, and the only place two of them touch the same directory — so it is
worth stating here rather than in either half.

`multiplai-context`'s lifecycle hooks maintain a **session registry** under
`<workspace>/.multiplai/data/sessions/`, one JSON entry per session, and its
fleet view aggregates that plus the per-session checkpoints into "which of my
sessions needs me". The gap no hook can close is that **a hook is code running
inside a session, so a session cannot report its own death**: only a clean exit
fires `SessionEnd`. A container killed by a reboot, a `docker kill`, the OOM
killer, or a closed terminal — routine under `docker run --rm` — leaves an entry
whose last event is whatever happened before it died, which reads exactly like an
agent waiting on you.

`multiplai-kit`'s `claude.sh` is the only observer standing outside the container
when it dies, so it leaves an empty `<session-id>.exited` marker beside the entry
once `docker run` returns; the plugin reads it as *ended* and clears it on the
session's next event.

**The rule that keeps it coherent: the plugin owns the JSON, and anything outside
a session leaves a one-bit marker file instead.** A launcher on a host with no
`jq`, or a hub mid-handshake, must never become a second writer of registry
state — that is how two stores start disagreeing silently.

Each half degrades alone: without the kit, uncleanly-killed sessions are listed
as idle until they age out; without the plugin, the marker is written and nobody
reads it. The full state model — every field, who writes it, and how each way of
stopping a session is detected — is documented once, in the
[multiplai-context README](https://github.com/spikelab/multiplai-cc-mktplace/blob/main/plugins/multiplai-context/README.md#session-accounting).

## Delivery contracts

Everything ships as **immutable tags** — merging to `main` alone delivers nothing.

- **Container → kit:** `release.sh` cuts an immutable container tag AND commits the
  `CONTAINER_REF` pin bump into the kit source. Tags are the unit of delivery; old tags
  are rollback points.
- **Kit → runtimes:** consumers update with `git pull && ./setup.sh`, which re-checks out
  the pinned container tag (shallow, detached-HEAD — never hand-edit it).
- **Plugins:** versioned in `marketplace.json`, tagged `<plugin>@<version>`, updated via
  Claude Code's `/plugin` menu.
- **Core → plugins:** each plugin script pins `multiplai-core@vX.Y.Z` in its PEP 723
  header (heavyweight pipelines pin via `uv.lock`); pins are bumped deliberately,
  per consumer.
