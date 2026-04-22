# fish-audit-prompt

![version](https://img.shields.io/badge/version-4.4.5-blue)
![license](https://img.shields.io/badge/license-MIT-green)
![fish](https://img.shields.io/badge/fish-3.4%2B-orange)
![checks](https://img.shields.io/badge/checks-374-brightgreen)

A structured, deterministic audit prompt for production Fish shell scripts managing system configuration, sudo, credentials, embedded configs, external profile loading, and argparse-dispatched modes with verification subsystems. Designed for use with Claude to perform exhaustive static and runtime analysis.

Derived from `ry-install.fish` v4.1.8 · 374 checks across 15 phases · Prompt v4.4.5

[changelog](CHANGELOG.md)

## Table of Contents

- [Metrics](#metrics)
- [Audit Structure](#audit-structure)
- [Phase Coverage](#phase-coverage)
- [Prerequisites](#prerequisites)
- [Usage](#usage)
- [Deliverables](#deliverables)
- [Files](#files)
- [License](#license)

## Metrics

| Metric | Value |
|---|---|
| Checks | 374 (363 static + 11 runtime) |
| Phases | 15 |
| Passes | 3 |

## Audit Structure

**PASS 1 — GATHER + ANALYZE** (phases 1–11, 13–15) — Parallel data collection via 7 hardened subshell groups, Python3 extractors, and rg pattern matching. Raw data to disk, findings appended, summary to context.

**PASS 2 — RUNTIME** (phase 12) — Fish execution tests covering stdout/stderr separation, argparse `--exclusive` enforcement, deprecated-flag rejection matrix, NO_COLOR compliance, redirect-bug regression, and SIGPIPE/signal exit-code help-text verification.

**PASS 3 — FINALIZE** — Collation, summary table, fixes spec with exact Before/After for `str_replace`, version bump, and zip packaging.

## Phase Coverage

| Phase | Checks | Scope |
|---|---|---|
| 1 | 12 | Fish compliance |
| 2 | 11 | stderr/stdout routing |
| 3 | 15 | Error handling, exit codes |
| 4 | 12 | Variables |
| 5 | 20 | Security |
| 6 | 30 | Embedded configs |
| 7 | 14 | Cross-references |
| 8 | 19 | Functions |
| 9 | 9 | Anti-patterns |
| 10 | 11 | Magic numbers, docs |
| 11 | 11 | Test/validate |
| 12 | 11 | Runtime |
| 13 | 5 | Gap analysis |
| 14 | 175 | Version-specific |
| 15 | 19 | Supplemental + profile invariants |

## Prerequisites

| Tool | Version | Fallback |
|---|---|---|
| fish | 3.4+ | PASS 2 skipped |
| python3 | 3.6+ | required |
| bash | 4.0+ | required |
| rg | ≥ 14.1.1 | `grep -P` |
| bat | any | `cat` |
| fd | any | `find` |
| fish_indent | bundled | DEFERRED |

> rg pinned ≥ 14.1.1 — Ubuntu 24.04 ships 14.1.0 with false-negative bug [#2884](https://github.com/BurntSushi/ripgrep/issues/2884). SETUP STEP 1 installs the pinned version.

## Usage

Provide this prompt to Claude along with the Fish script to audit. The prompt is self-contained: setup commands, data collection scripts, analysis instructions, checklist, and deliverable templates.

## Deliverables

Each audit produces:

- `audit-<script>-<version>.txt` — findings with severity, phase, check numbers, and line references
- `fixes-spec-<version>.txt` — actionable fix instructions with exact Before/After for `str_replace`

## Files

| File | Description |
|---|---|
| [`fish-audit-prompt.txt`](fish-audit-prompt.txt) | Audit prompt |
| [`CHANGELOG.md`](CHANGELOG.md) | History |
| [`README.md`](README.md) | This file |

## License

MIT
