# Changelog

All notable changes to this repository's documentation are recorded here.

This repo contains no code and carries no tags — the version numbers below mark
meaningful states of the documentation itself. Component releases are tracked in
each component repo; see the
[Releases section of the README](./README.md#releases).

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html)
as far as it applies to prose.

## [Unreleased]

### Added

- `ARCHITECTURE.md` gains **Runtime contract: who knows a session is over** —
  the session registry under `.multiplai/data/sessions/`, the only runtime
  state more than one repo touches. It states the constraint in one place
  rather than in either half: a hook runs inside a session, so a session
  cannot report its own death, and no observer outside it can tell a killed
  container from a tab left open. The suite's answer is to make the
  distinction not matter — quiet sessions are listed but never counted — and
  the rule that binds the hub is that only the plugin writes the JSON;
  everything else leaves a one-bit marker file. Detail stays in the plugin
  README.
- **README points at the kit's `GETTING-STARTED.md`** in two places — the end
  of "Start here" and the top of the "Docs" list. The front door told a new
  reader which command to run and then left them there; the guide is the thing
  that carries them from `setup.sh` to a working first session. The file ships
  from multiplai-kit, so that repo's change lands first.

### Changed

- **`ARCHITECTURE.md` no longer says plugin scripts pin core in a PEP 723
  header.** That convention was retired: the marketplace repo is now one `uv`
  workspace, each script directory that needs third-party dependencies is a
  member with its own `pyproject.toml`, the root `[tool.uv.sources]` names
  `multiplai-core` once and unpinned, and a single root `uv.lock` fixes the
  commit every member resolves to. The old wording described per-consumer pins
  that no longer exist — and the marketplace's own `lint_workspace.py` now
  rejects a PEP 723 header outright.

  Two other places said the retired thing and were missed on the first pass:
  the components table called core "consumed by plugin scripts via immutable
  git-tag pins", and the interlock diagram labelled that arrow
  `PEP-723 tag pins`. Both now name the lock.

  The bullet also **no longer claims Dependabot moves core.** It does not, and
  `multiplai-cc-mktplace`'s own `dependabot.yml` says why under a *Known gap*
  heading: Dependabot cannot bump a git-sourced dependency, and core is
  declared as a git URL. The weekly re-lock is real and covers third-party
  dependencies; core advances only when someone re-locks deliberately.

  Three README lines that leaned on the pinning idea were reworded with it:
  "the typed Python plumbing the plugin scripts pin", "the library API and its
  pinning contract", and "the tags plugin scripts pin".
- **"Six skill packs" is now seven, and "40+ skills" is 44.**
  `multiplai-apple` shipped on 2026-08-15 (the macOS-only `swift-build` split
  out of `multiplai-dev` so a Linux user is not carrying an Xcode skill), which
  left four statements here one short: two in `README.md`, two in
  `ARCHITECTURE.md`, plus the interlock diagram's "(7 plugins)". The skill
  count is counted, not estimated —
  `find plugins -name SKILL.md -path '*/skills/*' | wc -l` in the marketplace
  repo returns 44.
- **The host browser now has its own switch, and the security bullet says so.**
  The README already promised that bridge skills are enabled deliberately and
  "never by default"; until multiplai-container #23 that was aspiration for the
  browser, which the bridge turned on wholesale. It is now literal: the gateway
  refuses `agent-browser` unless a flag file exists on the Mac, and nothing in
  a container can create it. One clause in each of README and `ARCHITECTURE.md`
  — the mechanism stays in the container README.
- README tagline is now the suite's one-liner — "Your agent's model of you
  should be something you edited, not something that accreted while you
  weren't looking" — replacing "The first human-centered agentic harness";
  the closing sentence of the harness-comparison section was rewritten so it
  no longer duplicates the tagline.
- "Which part do you need?" adoption ladder reordered plugins-first
  (marketplace → container → kit → cockpit) in both README and
  `ARCHITECTURE.md`, with lead-ins updated to match; the marketplace-add
  command now uses the in-session `/plugin marketplace add` form everywhere.
- `ARCHITECTURE.md` no longer describes the environment as improving itself —
  it is "a persistent working environment that compounds — it learns what you
  approve".
- README "Releases" list repoints each component at its `CHANGELOG.md` on
  `main` (all four exist now); the `TODO(changelog-link)` comment is resolved
  and removed.
- README repositioned around the human-centered thesis: an opening on why automating the
  human out amplifies slop, an "Isn't this just another harness?" comparison
  (agent-curated memory vs. human-approved memory; Hermes claims verified
  against their live README 2026-07-26), a "No black boxes" observability
  bullet, an explicit conscious-opt-in caveat on the SSH bridge, and the
  gui roadmap note expanded with the mobile-native review argument.
- README and `ARCHITECTURE.md` now present `multiplai-gui` as *coming soon* and
  unlinked, instead of hyperlinking a private repository that 404s for visitors.
- README "Docs" section describes only artifacts that exist today; the promise of
  an mkdocs site deployed via GitHub Pages is gone.
- Skill count corrected and made growth-tolerant — "40+ skills, the memory engine
  plus six skill packs" (was "~35 skills in 7 plugin packs"), pointing at the
  `multiplai-cc-mktplace` compatibility matrix as the authoritative list.

### Added

- README `## Community` section — Discussions on this repo as the suite's
  community home; bugs go to component-repo issues.
- `SECURITY.md` — reporting contact (`security@spikelab.org`) scoped to this
  repo's docs-and-coordination role, pointing component vulnerabilities at
  the component repos.
- `CLAUDE.md` — working rules for agents editing this repo: no code lives here,
  counts must be re-derived from the source repos, `multiplai-gui` stays unlinked
  until it ships.
- `CHANGELOG.md` (this file).
- README "Start here" section with the one command a first-time visitor can paste.
- README "Docs" → "Releases" subsection linking each component's release record.

## [0.1.0] – 2026-07-26

### Added

- Bootstrap of the umbrella repo (`881a6af`): front-door `README.md` and the
  canonical suite `ARCHITECTURE.md` that the component repos link to instead of
  carrying their own copies.
- MIT `LICENSE`, matching `multiplai-core` and `multiplai-container` (`454823b`).
