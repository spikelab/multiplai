# CLAUDE.md — spikelab/multiplai (umbrella)

**This repo contains no code.** It is the suite's front door (`README.md`) and the
canonical `ARCHITECTURE.md`. The five component repos link here rather than
carrying their own copies of the architecture — so an edit to `ARCHITECTURE.md`
changes what every component repo points at. Treat it accordingly.

Files: `README.md`, `ARCHITECTURE.md`, `CHANGELOG.md`, `CLAUDE.md`, `LICENSE`,
`.gitignore`. There is no build, no test suite, and no CI.

## Never state a count from memory

Skill, plugin, and reference-doc counts drift constantly and have been wrong here
before. Re-derive them from the source repos *before* editing any sentence that
contains a number:

```bash
# skills across all plugins
ls -d PROJECTS/multiplai-cc-mktplace/plugins/*/skills/*/ | wc -l

# per plugin
for d in PROJECTS/multiplai-cc-mktplace/plugins/*/; do \
  echo "$(basename $d): $(ls -d $d/skills/*/ | wc -l)"; done

# reference docs
ls PROJECTS/multiplai-kit/dotfiles/reference/dev/*.md | wc -l
```

Prefer growth-tolerant phrasing ("40+ skills") over an exact number that will
drift again, and name `multiplai-cc-mktplace`'s compatibility matrix as the
authoritative list. Note that `multiplai-context` is a plugin but not a *skill
pack* — the correct framing is "the memory engine plus six skill packs".

## `multiplai-gui` is private and unreleased

Do not link `github.com/spikelab/multiplai-gui` anywhere — the repo is private
and any hyperlink 404s for visitors. It may be described as *coming soon* /
*not yet released*, unlinked, until Spike says it ships.

## No unbuilt feature in the present or near-future tense

Everything this repo describes must exist today. No "is being built", no
"deployed via", no timelines. A bare "may follow" is acceptable; a promise is
not. In particular: there is no docs site, no `docs/` directory, and no GitHub
Pages workflow — do not scaffold one and do not imply one exists.

## Changelog convention

`CHANGELOG.md` follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
with `## [Unreleased]` at the top and Added/Changed/Fixed/Removed subsections.
Same convention across the suite. This repo has no tags; its version headings mark
documentation states only. Component release records live in the component repos.
