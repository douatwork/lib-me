# Changelog

All notable changes to lib-me are recorded here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).
While lib-me is pre-1.0, the structure may change between minor versions.

## [Unreleased]

## [0.1.0] - 2026-06-09

First public release.

### Added
- Four-layer structure: `1-core/` (identity), `2-stack/` (machine & tooling),
  `3-taste/` (voice, code style, naming, sample corpus), `4-workflows/`
  (agent bootstrap, project setup, work management).
- `4-workflows/agents-init.md` — the bootstrap spec any runtime follows to find
  lib-me, navigate it, and keep its derived skills in sync via commit-hash anchoring.
- Human-facing `readme.md` with the vision, the map, and a quickstart.
- `contributing.md` — how to adopt lib-me and wire a new agent runtime.
- MIT `license`.
- Neutral demonstration corpus under `3-taste/corpus/` (creative, doc, post, email)
  and a showcase voice guide in `3-taste/writing-style.md`.

[Unreleased]: https://github.com/douatwork/lib-me/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/douatwork/lib-me/releases/tag/v0.1.0
