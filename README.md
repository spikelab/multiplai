# Multiplai

> Your agent's model of you should be something you edited, not something that accreted while you weren't looking.

Every agent harness right now is racing in the same direction: automate the
human out. Longer autonomous runs, more parallel agents, memory the agent
curates for itself, instructions it rewrites on its own. It sounds great,
until you actually try it. An agent that doesn't really know you produces the
average answer: the code everyone writes, the prose nobody wanted. Take
yourself out of the loop and you're not scaling your judgment. You're
amplifying slop.

Here's the uncomfortable part: staying in the loop is work. Reviewing what ten
sessions did overnight, correcting the same taste mistake twice, deciding what
your agent should remember about you — at scale it's exhausting, and that
exhaustion is exactly why everyone else concluded the human has to go.

Multiplai makes the opposite bet. The human is the value to amplify, so the
tooling should make being the human easier, and honestly, more pleasant.
Capture everything automatically; spend your attention only where your
judgment matters; make the reviewing itself fast enough that you'll actually
do it.

It starts with context, because an agent that doesn't know you can't do
anything else well.

## What that looks like in practice

- **Memory that compounds, with you as editor.** Sessions keep a diary and
  capture learnings on their own — that's the taxing part, automated. But
  nothing is promoted to long-term memory without your sign-off: a review
  step ("dreams") proposes consolidations and you decide which ones make it
  in, because that curation is what shapes everything the agent does next.
  Relevant context is then routed into every new session automatically.
  Week 40 knows what week 1 learned, and you know why.
- **No black boxes.** Every routing decision, memory write, and hook run
  leaves a standardized, human-readable log line (with a structured trace
  mirror). When the agent did something surprising, you can see what
  happened, verify it, and steer — instead of guessing at a vibe.
- **Autonomy that's actually safe to grant.** Sessions run
  `--dangerously-skip-permissions` inside a Docker container that *is* the
  permission boundary. A key-restricted SSH bridge exists for the few tools
  that genuinely need your Mac (Xcode, Whisper, your real Chrome), and every
  bridge skill you enable widens that boundary: you enable them deliberately,
  knowing what each opens up, never by default. You stop babysitting prompts
  because the walls are real and you know exactly where the doors are.
- **40+ skills — the memory engine plus six skill packs.** Autonomous
  spec-driven builds, deep research, transcription, Slack and Gmail, PM
  artefacts, long-form writing. Each one encodes *how* the work should be
  done, so your standards travel with the task. The authoritative list is the
  compatibility matrix in the
  [multiplai-cc-mktplace README](https://github.com/spikelab/multiplai-cc-mktplace#compatibility-matrix).

On the roadmap, not yet released: **multiplai-gui**, a native macOS/iOS
cockpit for observing, driving, and triaging many sessions at once (a session
is a file, so watching is free). Everyone else ports the same chat box to a
smaller screen; what you want to do on your phone is different in kind —
glance at what your agents did, swipe through what they learned, approve or
redirect in seconds, put it back in your pocket. Nobody else is designing for
that, because nobody else is optimizing for the human doing the reviewing.

## Isn't this just another harness?

Same species, opposite bet. The systems getting attention today put the agent
in charge of its own evolution:

- Companion agents in the OpenClaw / Hermes mold let the agent curate its own
  memory and grow its own skills — OpenClaw goes further and rewrites its own
  operating instructions. Run one for a month and its picture of you is
  whatever it decided to keep.
- Other memory pipelines compile knowledge through an autonomous LLM
  librarian. Well-engineered, provenance-tracked, and still: the trust sits
  with the compiler, not with you.

Multiplai automates the capture and keeps the judgment. Everything gets
recorded; nothing becomes memory without your approval; every decision the
system makes is logged where you can read it; and the review is engineered to
cost you minutes, not evenings.

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
     repoint these at the files themselves — the `blob/main/CHANGELOG.md` path in
     each component repo. -->

- [multiplai-container releases](https://github.com/spikelab/multiplai-container/releases) — the tag consumed by the kit's `CONTAINER_REF` pin
- [multiplai-core releases](https://github.com/spikelab/multiplai-core/releases) — the tags plugin scripts pin
- [multiplai-cc-mktplace releases](https://github.com/spikelab/multiplai-cc-mktplace/releases) — per-plugin `<plugin>@<version>` tags
- [multiplai-kit releases](https://github.com/spikelab/multiplai-kit/releases) — untagged so far; the kit is consumed by `git pull && ./setup.sh`

## Status

Young, personal, built in public. I run my whole working life on it — that's
both the pitch and the disclaimer. Expect fast movement on `main` everywhere;
what's released is what's tagged.
