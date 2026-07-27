# Security

This repository contains no code — it is the suite's front door (`README.md`)
and the canonical `ARCHITECTURE.md`. There is nothing here to exploit directly,
but it is still the right place to report two things: a vulnerability you can't
place in a specific component, and anything wrong with what these documents
claim (a link that now points somewhere it shouldn't, instructions that would
lead a reader to do something unsafe).

## Reporting a vulnerability

Email **security@spikelab.org**. Please include:

- which repository the issue lives in, if you know it — component
  vulnerabilities are best reported against the component:
  [multiplai-cc-mktplace](https://github.com/spikelab/multiplai-cc-mktplace/blob/main/SECURITY.md)
  has its own SECURITY.md covering the plugins and skills; issues in
  [multiplai-core](https://github.com/spikelab/multiplai-core),
  [multiplai-kit](https://github.com/spikelab/multiplai-kit), or
  [multiplai-container](https://github.com/spikelab/multiplai-container) reach
  the same address — say which repo;
- what an attacker gets, and the shortest reproduction you have.

Do **not** open a public issue for something exploitable. Anything else — a
broken link, a doc that contradicts a component README — is a normal issue and
is welcome as one.

Expect a first reply within a few days. This is a small project maintained by
one person; there is no bounty and no SLA, and saying so is more useful than
implying otherwise.

## Scope

In scope: the documents in this repository and the coordination they do —
where they send readers, what they tell readers to run. Out of scope: the
components themselves (use the component repo's SECURITY.md where one exists,
or the same email address either way) and Claude Code itself (report to
Anthropic).
