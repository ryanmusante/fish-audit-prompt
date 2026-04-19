# fish-audit-prompt

![version](https://img.shields.io/badge/version-4.0.0-blue)
![license](https://img.shields.io/badge/license-MIT-green)
![fish](https://img.shields.io/badge/fish-3.4%2B-orange)
![checks](https://img.shields.io/badge/checks-357-brightgreen)

A structured, deterministic audit prompt for production Fish shell scripts managing system configuration, sudo, credentials, embedded configs, external profile loading, and argparse-dispatched modes with verification subsystems. Designed for use with Claude to perform exhaustive static and runtime analysis.

Derived from `ry-install.fish` v4.1.3 · 357 checks across 16 phases · Prompt v4.0.0

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
| Total checks | 357 (346 static + 11 runtime) |
| Phases | 16 (11 static · 1 runtime · 1 gap analysis · 1 version-specifics · 1 supplemental · 1 profile-system) |
| Passes | 3 (gather+analyze · runtime · finalize) |
| Retired checks | 168–170 (backup/rollback/--dry-run, ry-install v3.5.0–v4.1.0) |
| Added checks | 334–367 (v3.47.0–v4.1.3: source-safety, profile system, argparse dispatcher, _tmpfile_key, SIG-prefix signal matching, SYU/BOOT_WIPE ack gates, nftables firewall, etc.) |

## Audit Structure

**PASS 1 — GATHER + ANALYZE** (phases 1–11, 13–15) — Parallel data collection via 7 hardened subshell groups, Python3 extractors, and rg pattern matching. Raw data to disk, findings appended, summary to context.

**PASS 2 — RUNTIME** (phase 12) — Fish execution tests covering stdout/stderr separation, argparse `--exclusive` enforcement, deprecated-flag rejection matrix, NO_COLOR compliance, redirect-bug regression, and SIGPIPE/signal exit-code help-text verification.

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
| 11 | 11 | Testing and validation |
| 12 | 11 | Runtime tests |
| 13 | 5 | Gap analysis |
| 14 | 158 | Version-specific (v3.1.0–v4.1.3 + Fish 4.0–4.6 compat) |
| 15 | 19 | Supplemental deep checks + profile-system invariants |

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
| [`fish-audit-prompt.txt`](fish-audit-prompt.txt) | Complete audit prompt (v4.0.0) |
| [`CHANGELOG.md`](CHANGELOG.md) | Version history |
| [`LICENSE`](LICENSE) | MIT license |
| [`README.md`](README.md) | This file |

## License

[MIT](LICENSE)
