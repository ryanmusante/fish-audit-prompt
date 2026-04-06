# fish-audit-prompt

![version](https://img.shields.io/badge/version-3.8.1-blue?style=flat-square)
![license](https://img.shields.io/badge/license-MIT-green?style=flat-square)
![fish](https://img.shields.io/badge/fish-3.4%2B-orange?style=flat-square)
![checks](https://img.shields.io/badge/checks-321-brightgreen?style=flat-square)

A structured, deterministic audit prompt for production Fish shell scripts managing system configuration, sudo, credentials, embedded configs, and multi-mode dispatch with verification subsystems. Designed for use with Claude to perform exhaustive static and runtime analysis.

Derived from `ry-install.fish` v3.44.0 · 321 checks across 15 phases · Prompt v3.8.1

[changelog](CHANGELOG.md)

## Table of Contents

- [Metrics](#metrics)
- [Audit Structure](#audit-structure)
- [Phase Coverage](#phase-coverage)
- [Prerequisites](#prerequisites)
- [Usage](#usage)
- [Deliverables](#deliverables)
- [Files](#files)
- [Version History](#version-history)
- [License](#license)

## Metrics

| Metric | Value |
|---|---|
| Total checks | 321 (306 static + 15 runtime) |
| Phases | 15 (12 static · 1 runtime · 1 gap analysis · 1 version-specifics) |
| Passes | 3 (gather+analyze · runtime · finalize) |
| Audit infra lessons | 4 (critical infrastructure traps) |

## Audit Structure

**PASS 1 — GATHER + ANALYZE** (phases 1–11, 13–15) — Parallel data collection via 7 hardened subshell groups, Python3 extractors, and rg pattern matching. Raw data to disk, findings appended, summary to context.

**PASS 2 — RUNTIME** (phase 12) — Fish execution tests covering stdout/stderr separation, exit code matrix, NO_COLOR enforcement, dry-run filesystem safety, and internal consistency.

**PASS 3 — FINALIZE** — Collation, summary table, fixes spec with exact Before/After for `str_replace`, version bump, and zip packaging.

## Phase Coverage

| Phase | Checks | Scope |
|---|---|---|
| 1 | 12 | Fish shell compliance |
| 2 | 11 | Stderr/stdout routing |
| 3 | 15 | Error handling and exit codes |
| 4 | 12 | Variable handling |
| 5 | 20 | Security |
| 6 | 30 | Embedded config content |
| 7 | 14 | Cross-references and consistency |
| 8 | 19 | Function architecture |
| 9 | 9 | Patterns and anti-patterns |
| 10 | 11 | Magic numbers and documentation |
| 11 | 14 | Testing and validation |
| 12 | 15 | Runtime tests |
| 13 | 5 | Gap analysis |
| 14 | 115 | Version-specific (v3.1.0–v3.44.0 + Fish 4.0–4.6 compat) |
| 15 | 19 | Supplemental deep checks |

## Prerequisites

| Tool | Version | Purpose | Fallback |
|---|---|---|---|
| fish | 3.4+ | Runtime tests (PASS 2), syntax validation | PASS 2 skipped if < 3.4 |
| python3 | 3.6+ | Function body extraction, depth-tracking analysis | None (required) |
| bash | 4.0+ | Data collection scripts, SETUP fallbacks | None (required) |
| rg (ripgrep) | ≥ 14.1.1 | Pattern matching across all phases | `grep -P` (auto-fallback) |
| bat | any | Paged file display during analysis | `cat` (auto-fallback) |
| fd | any | File discovery | `find` (auto-fallback) |
| fish_indent | bundled | Check 215 canonical formatting | DEFERRED if missing |

> **Note:** rg is pinned to ≥ 14.1.1. Ubuntu 24.04 ships 14.1.0 which has a confirmed false-negative bug ([BurntSushi/ripgrep#2884](https://github.com/BurntSushi/ripgrep/issues/2884)). SETUP STEP 1 installs the pinned version automatically.

## Usage

Provide this prompt to Claude along with the Fish script to audit. The prompt is self-contained: setup commands, data collection scripts, analysis instructions, checklist, and deliverable templates.

## Deliverables

Each audit produces:

- `audit-<script>-<version>.txt` — findings with severity, phase, check numbers, and line references
- `fixes-spec-<version>.txt` — actionable fix instructions with exact Before/After for `str_replace`

## Files

| File | Description |
|---|---|
| [`fish-audit-prompt.txt`](fish-audit-prompt.txt) | Complete audit prompt (v3.8.1) |
| [`CHANGELOG.md`](CHANGELOG.md) | Version history |
| [`LICENSE`](LICENSE) | MIT license |
| [`README.md`](README.md) | This file |

## Version History

| Version | Checks | Date | Summary |
|---|---|---|---|
| v3.8.1 | 321 | 2026-04-05 | +20 v3.10–v3.44 checks; _ry_ prefix sync; remove KEY LESSONS; sync ry-install v3.44.0 |
| v3.8.0 | 301 | 2026-04-05 | +7 Fish 4.x checks; hardening preamble; rg 14.1.1 pin |
| v3.7.32 | 294 | 2026-03-18 | +1 check: sudo -n credential safety |
| v3.7.31 | 293 | 2026-03-18 | 2 fixes: _py_extract dedup, VERSION guard |
| v3.7.30 | 293 | 2026-03-18 | 2 fixes: PASS3 collation, ANSI PCRE2 |
| v3.7.29 | 293 | 2026-03-17 | 5 fixes: grep fallback, budget, timeout, stdout, glob |
| v3.7.28 | 293 | 2026-03-17 | 4 fixes: sync, help verify, flag equiv, scope |
| v3.7.27 | 293 | 2026-03-17 | 3 fixes: dry-run, GROUP E dedup, infra lesson |
| v3.7.26 | 293 | 2026-03-17 | 6 fixes: SIZE GATE, dry-run FS, rg, timeout, sort |
| v3.7.25 | 293 | 2026-03-17 | 4 fixes: data map, GROUP_EXIT, severity, dedup |
| v3.7.24 | 293 | 2026-03-17 | 3 fixes: escaped pipe rg patterns, README count |
| v3.7.23 | 293 | 2026-03-17 | +8 checks, 16 GROUP D patterns |
| v3.7.13 | 285 | 2026-03-16 | Unified versioning, 12 fixes |

See [`CHANGELOG.md`](CHANGELOG.md) for full details.

## License

[MIT](LICENSE)
