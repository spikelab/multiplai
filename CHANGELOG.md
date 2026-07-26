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

### Changed

- README repositioned around the human-centered thesis: new tagline ("The
  first human-centered agentic harness"), an opening on why automating the
  human out amplifies slop, an "Isn't this just another harness?" comparison
  (agent-curated memory vs. human-approved memory; Hermes claims verified
  against their live README 2026-07-26), a "No black boxes" observability
  bullet, an explicit conscious-opt-in caveat on the SSH bridge, and the
  gui roadmap note expanded with the mobile-native review argument.

### Added

- `CLAUDE.md` — working rules for agents editing this repo: no code lives here,
  counts must be re-derived from the source repos, `multiplai-gui` stays unlinked
  until it ships.
- `CHANGELOG.md` (this file).
- README "Start here" section with the one command a first-time visitor can paste.
- README "Docs" → "Releases" subsection linking each component's release record.

### Changed

- README and `ARCHITECTURE.md` now present `multiplai-gui` as *coming soon* and
  unlinked, instead of hyperlinking a private repository that 404s for visitors.
- README "Docs" section describes only artifacts that exist today; the promise of
  an mkdocs site deployed via GitHub Pages is gone.
- Skill count corrected and made growth-tolerant — "40+ skills, the memory engine
  plus six skill packs" (was "~35 skills in 7 plugin packs"), pointing at the
  `multiplai-cc-mktplace` compatibility matrix as the authoritative list.

## [0.1.0] – 2026-07-26

### Added

- Bootstrap of the umbrella repo (`881a6af`): front-door `README.md` and the
  canonical suite `ARCHITECTURE.md` that the component repos link to instead of
  carrying their own copies.
- MIT `LICENSE`, matching `multiplai-core` and `multiplai-container` (`454823b`).
