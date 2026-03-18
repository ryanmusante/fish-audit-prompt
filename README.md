# fish-audit-prompt

![version](https://img.shields.io/badge/version-3.7.32-blue?style=flat-square)
![license](https://img.shields.io/badge/license-MIT-green?style=flat-square)
![fish](https://img.shields.io/badge/fish-3.4%2B-orange?style=flat-square)

Derived from `ry-install.fish` v3.7.23 · 294 checks across 15 phases · Prompt v3.7.32

## Overview

A structured, deterministic audit prompt for production Fish shell scripts managing system configuration, sudo, credentials, embedded configs, and multi-mode dispatch with verification subsystems. Designed for use with Claude to perform exhaustive static and runtime analysis.

## Metrics

| Metric | Value |
|--------|-------|
| Total checks | 294 (279 static + 15 runtime) |
| Phases | 15 (12 static + 1 runtime + 1 gap analysis + 1 version-specifics) |
| Passes | 3 (gather+analyze, runtime, finalize) |
| Key lessons | 60+ (indexed by check number) |
| Audit infra lessons | 40+ (indexed by prompt section) |

## Audit Structure

**PASS 1 — GATHER + ANALYZE** (phases 1–11, 13–15): parallel data collection via 7 subshell groups, python3 extractors, and rg pattern matching. Raw data to disk, findings appended, summary to context.

**PASS 2 — RUNTIME** (phase 12): fish execution tests covering stdout/stderr separation, exit code matrix, NO_COLOR enforcement, dry-run filesystem safety, and internal consistency (lint, test-all).

**PASS 3 — FINALIZE**: collation, summary table, fixes spec with exact Before/After for `str_replace`, version bump, and zip packaging.

## Phase Coverage

| Phase | Checks | Scope |
|-------|--------|-------|
| 1 | 12 | Fish shell compliance (bashisms, version gate, string ops, argparse) |
| 2 | 11 | Stderr/stdout routing (output contract, message functions, NO_COLOR) |
| 3 | 15 | Error handling & exit codes (signals, cleanup, status capture) |
| 4 | 12 | Variable handling (scope, set --erase, naming, quoting) |
| 5 | 20 | Security (injection, temp files, credentials, privileges) |
| 6 | 30 | Embedded config content (systemd-boot, cmdline, udev, IWD, NM, sysctl) |
| 7 | 14 | Cross-references & consistency (version sync, file counts, README) |
| 8 | 19 | Function architecture (arity, atomic ops, logging, lock file) |
| 9 | 9 | Patterns & anti-patterns (false positives, dead code, style) |
| 10 | 11 | Magic numbers & documentation (named constants, help, changelog) |
| 11 | 14 | Testing & validation (preflight, config validation, dry-run) |
| 12 | 15 | Runtime tests (stdout/stderr, exit codes, NO_COLOR, dry-run, lint) |
| 13 | 5 | Gap analysis (race conditions, error paths, DRY gates, lock) |
| 14 | 88 | v3.1.0–v3.7.32 specifics (KVER, helpers, sysctl, scope, decomposition, batch, parallel, injection safety, sudo -n) |
| 15 | 19 | Supplemental deep checks (formatting, tmpfile tracing, flag sync) |

## Files

| File | Description |
|------|-------------|
| `fish-audit-prompt.txt` | The complete audit prompt (v3.7.32) |
| `CHANGELOG.txt` | Version history with per-finding details |
| `README.md` | This file |

## Deliverables

Each audit produces:

- `audit-<script>-<version>.txt` — full findings with severity, phase, check numbers, and line references
- `fixes-spec-<version>.txt` — actionable fix instructions with exact Before/After text for `str_replace`

## Version History

| Version | Checks | ry-install | Summary |
|---------|--------|------------|---------|
| v3.7.32 | 294 | v3.7.32 | +1 check: _find_pacnew_files sudo -n non-interactive credential safety |
| v3.7.31 | 293 | v3.7.23 | 2 fixes: duplicate _py_extract batch processing, PASS2 VERSION multi-line guard |
| v3.7.30 | 293 | v3.7.23 | 2 fixes: PASS3 collation double-output on 0-match severities, ANSI check unnecessary PCRE2 flag |
| v3.7.29 | 293 | v3.7.23 | 5 fixes: TOTAL grep fallback, context budget label, test-all timeout, op mode stdout coverage, empty section guard glob |
| v3.7.28 | 293 | v3.7.23 | 4 fixes: CHANGELOG sync, help stdout verify, flag equivalence, scope shadow begin boundary |
| v3.7.27 | 293 | v3.7.23 | 3 fixes: PASS2 dry-run/stdout analysis, GROUP E redundancy removal, infra lesson |
| v3.7.26 | 293 | v3.7.23 | 6 fixes: SIZE GATE label, dry-run FS coverage, rg fallback, timeout doc, P3 sort, Group C labels |
| v3.7.25 | 293 | v3.7.23 | 4 fixes: data mapping, GROUP_EXIT verify, severity def, CHANGELOG dedup |
| v3.7.24 | 293 | v3.7.23 | 2 rg pattern fixes (AND-OR escaped pipe), README group count fix |
| v3.7.23 | 293 | v3.7.23 | +8 checks, 16 new GROUP D patterns, 5 new infra lessons |
| v3.7.13 | 285 | v3.7.13 | Unified versioning, 6 data collection bug fixes |
| v25–v31.2.1 | 254–285 | v2.x–v3.7.1 | 22 releases |

See [`CHANGELOG.txt`](CHANGELOG.txt) for per-finding details.

## Prerequisites

| Tool | Version | Purpose | Fallback |
|------|---------|---------|----------|
| fish | 3.4+ | Runtime tests (PASS 2), syntax validation | PASS 2 skipped if < 3.4 |
| python3 | 3.6+ | Function body extraction, depth-tracking analysis | None (required) |
| bash | 4.0+ | Data collection scripts, SETUP fallbacks | None (required) |
| rg (ripgrep) | any | Pattern matching across all phases | `grep -P` (auto-fallback) |
| bat | any | Paged file display during analysis | `cat` (auto-fallback) |
| fd | any | File discovery | `find` (auto-fallback) |
| fish_indent | (bundled with fish) | Check 215 canonical formatting | DEFERRED if missing |

SETUP STEP 1 installs fish, ripgrep, fd-find, and bat via apt. STEP 2 defines bash function fallbacks for rg, bat, and fd when binaries are unavailable.

## Usage

Provide this prompt to Claude along with the Fish script to audit. The prompt is self-contained: it includes setup commands, data collection scripts, analysis instructions, checklist, and deliverable templates.

## License

MIT
