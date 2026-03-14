# fish-audit-prompt

Version **31.2.0** · Derived from `ry-install.fish` v3.7.1 · 285 checks across 15 phases

## Overview

A structured, deterministic audit prompt for production Fish shell scripts managing system configuration, sudo, credentials, embedded configs, and multi-mode dispatch with verification subsystems. Designed for use with Claude to perform exhaustive static and runtime analysis.

## Metrics

| Metric | Value |
|--------|-------|
| Total checks | 285 (270 static + 15 runtime) |
| Phases | 15 (12 static + 1 runtime + 1 gap analysis + 1 version-specifics) |
| Passes | 3 (gather+analyze, runtime, finalize) |
| Key lessons | 30+ (indexed by check number) |
| Audit infra lessons | 15+ (indexed by prompt section) |
| Changelog versions | 23 |

## Audit Structure

**PASS 1 — GATHER + ANALYZE** (phases 1–11, 13–15): parallel data collection via 5 subshell groups, python3 extractors, and rg pattern matching. Raw data to disk, findings appended, summary to context.

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
| 14 | 79 | v3.1.0–v3.7.1 specifics (KVER, helpers, sysctl, scope, decomposition, batch, parallel) |
| 15 | 19 | Supplemental deep checks (formatting, tmpfile tracing, flag sync) |

## Files

| File | Description |
|------|-------------|
| `fish-audit-prompt.txt` | The complete audit prompt (v31.2.0) |
| `CHANGELOG.txt` | Version history with per-finding details |
| `README.md` | This file |

## Deliverables

Each audit produces:

- `audit-<script>-<version>.txt` — full findings with severity, phase, check numbers, and line references
- `fixes-spec-<version>.txt` — actionable fix instructions with exact Before/After text for `str_replace`

## Version History

See `CHANGELOG.txt` for the complete record. Key milestones:

- **v31.2.0** — Exhaustive self-audit: fixed scope shadow nested function bug, _py_extract bare declaration regex, PASS 2 STEP 2 undefined $S, corrected extractor list/count, fixed bare coreutils word boundary filter; 4 new audit infra lessons
- **v31.1.0** — Self-audit: clarified check 1 $() as style enforcement (valid Fish 3.0+), documented single-line begin/end extractor limitation, added inline comment density total count, 3 new audit infra lessons
- **v31.0.0** — Sync with ry-install v3.7.1: 10 new checks (279-288) for kernel gate return, PROGRESS_STEPS ordering, nested parallelism guard, ssh-agent --user verify, dead var removal, _ensure_sudo_cached consistency, batch enable fallback, manifest success gate, syu ordering, flock rotation; corrected check 201 (28 section comments) and 262 (33 functions >50L); updated 4 metric checks
- **v30.0.0** — Deep verification of v3.7.0 parallel patterns: 6 new checks for _TRACKED_TMPFILES lifecycle, functions serialization, parallel env export, string join0, _parse_systemctl_show, result file determinism; 7 new data collection patterns; stale numeric vars removed; command prefix ≥74, flock ≥10
- **v29.0.0** — Sync with ry-install v3.7.0: 6 new checks for flock lock reclaim, _ROOT_UUID cache, _kconfig_cache, fish 3.4+ gate, batch/parallel processing; updated metrics (92 functions, 703 set -l, 35 numeric vars); check 129 updated for legitimate flock usage
- **v28.11.0** — LOG TIMESTAMP pattern updated from stale `_TS_CACHE` to `date`; STDOUT LEAKS `>>` exclusion; MSG_SKIP `_ask` addition
- **v28.10.0** — NUMERIC VAR INIT expanded 17→34 local vars; centralized MSG_SKIP; SIZE GATE recovery manifest; resume guard; HAS_LOCK DISPATCH removed
- **v28.9.0** — SIZE GATE p14 label aligned with STEP 2 mapping (P14-P15)
- **v28.8.0** — DRY GATE skip list aligned with lesson (_log, printf); --test-all exit code added to PASS 2 matrix
- **v28.7.0** — BARE COREUTILS and DRY GATE skip lists expanded for message functions; cross-validated against ry-install v3.5.2
- **v28.4.0** — Scope shadow detector escape fix, `_py_extract` inner-end fix, `open('$F')` → `sys.argv[1]` migration
- **v28.2.0** — `_py_extract` depth tracking fix (block_re), TMPFILE LIFECYCLE extractor fix
- **v26.0.0** — Phase reordering, PCRE2/multiline fallbacks, context budget management
- **v25** — Removed watch/rollback/gaming/profile/interval modes (v3.3.2 feature removal)

## Usage

Provide this prompt to Claude along with the Fish script to audit. The prompt is self-contained: it includes setup commands, data collection scripts, analysis instructions, checklist, and deliverable templates.

## License

MIT
