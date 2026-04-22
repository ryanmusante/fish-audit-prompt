# fish-audit-prompt

![version](https://img.shields.io/badge/version-4.5.0-blue)
![license](https://img.shields.io/badge/license-MIT-green)
![fish](https://img.shields.io/badge/fish-3.4%2B-orange)
![checks](https://img.shields.io/badge/checks-375-brightgreen)

A structured, deterministic audit prompt for production Fish shell scripts managing system configuration, sudo, credentials, embedded configs, external profile loading, and argparse-dispatched modes with verification subsystems. Designed for use with Claude to perform exhaustive static and runtime analysis.

Derived from `ry-install.fish` v4.1.8 · 375 checks across 15 phases · Prompt v4.5.0

[changelog](CHANGELOG.md)

## Table of Contents

- [Metrics](#metrics)
- [Audit Structure](#audit-structure)
- [Phase Coverage](#phase-coverage)
- [Prerequisites](#prerequisites)
- [Usage](#usage)
- [Security Considerations](#security-considerations)
- [Deliverables](#deliverables)
- [Files](#files)
- [License](#license)

## Metrics

| Metric | Value |
|---|---|
| Checks | 375 (364 static + 11 runtime) |
| Phases | 15 |
| Passes | 3 |

## Audit Structure

**PASS 1 — GATHER + ANALYZE** (phases 1–11, 13–15) — Parallel data collection via 7 hardened subshell groups, Python3 extractors, and rg pattern matching. Raw data to disk, findings appended, summary to context.

**PASS 2 — RUNTIME** (phase 12) — Fish execution tests covering stdout/stderr separation, argparse `--exclusive` enforcement, deprecated-flag rejection matrix, NO_COLOR compliance (both `NO_COLOR=1` suppression and `NO_COLOR=` empty non-suppression per no-color.org v1.0), redirect-bug regression, and SIGPIPE/signal exit-code help-text verification.

**PASS 3 — FINALIZE** — Collation via single-pass awk, summary table, fixes spec with exact Before/After for `str_replace`, version bump, and zip packaging.

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
| 14 | 176 | Version-specific |
| 15 | 19 | Supplemental + profile invariants |

## Prerequisites

| Tool | Version | Fallback |
|---|---|---|
| fish | 3.4+ | PASS 2 skipped |
| python3 | 3.6+ | required |
| bash | 4.0+ | required (hard-enforced at SETUP STEP 2) |
| rg | ≥ 14.1.1 (pins to 15.0.1 if older) | hard-fail (no `grep -P` degrade mode) |
| awk | POSIX | required (PASS 1 EMPTY SECTION GUARD + PASS 3 collation) |
| curl | any | required (SETUP STEP 1 rg pin download) |
| zip | any | required (PASS 3 STEP 4 packaging) |
| bat | any | `cat` |
| fd | any | `find` |
| fish_indent | bundled | DEFERRED |

> rg pinned ≥ 14.1.1 — Ubuntu 24.04 ships 14.1.0 with false-negative bug [#2884](https://github.com/BurntSushi/ripgrep/issues/2884). SETUP STEP 1 installs rg 15.0.1 if a non-conforming version is present.
> Package installer auto-detects `apt-get` (Debian/Ubuntu), `pacman` (Arch/CachyOS), and `dnf` (Fedora/RHEL). Any other host must install `fish fd bat ripgrep` manually before running.

## Usage

Provide this prompt to Claude along with the Fish script to audit. The prompt is self-contained: setup commands, data collection scripts, analysis instructions, checklist, and deliverable templates.

## Security Considerations

The audit prompt is designed to run with minimal privilege even when the operator runs it as root. Claude relies on the following hardening invariants — **do not strip them**:

- **Environment hardening (SETUP STEP 2)** — `LC_ALL=C.UTF-8` (reproducible coreutils), `PYTHONUTF8=1` (deterministic Python I/O), `RIPGREP_CONFIG_PATH=""` (blocks `.ripgreprc` injection into rg behavior), `PATH` sanitized to `/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin` (includes `/usr/sbin` so `runuser` resolves on Debian/Ubuntu), `umask 077` (owner-only temp files), and `unset CDPATH BASH_ENV ENV` (blocks implicit environment execution).
- **Bash-only SETUP contract** — STEP 2 hard-aborts with exit 2 if invoked under a non-bash shell; the shim block uses arrays, `${args[-1]}`, and `${range%%:*}` which POSIX `sh`/`dash` do not honor.
- **PASS 2 root-handling** — if `id -u` is 0, Claude drops privileges via `runuser -u $PASS2_USER` (default `nobody`; override via `PASS2_FISH_USER` env). The per-run `$PASS2_HOME` tmpdir under `/tmp` is owned by that user at mode `0700`. This is a **required** safety invariant, not optional — fish sources `~/.config/fish/config.fish` even in non-interactive invocations, so a writable `$PASS2_HOME` would permit a local attacker to race `config.fish` into place between `mkdir` and the first fish spawn, yielding arbitrary code execution as `$PASS2_USER`.
- **Cleanup trap** — the `$PASS2_HOME` cleanup trap covers `EXIT INT TERM HUP` (not just `EXIT`) so Ctrl-C during the 4-parallel fish spawn still removes the tmpdir.
- **Invocation rule** — **do not** invoke the prompt via `sudo bash prompt.sh`. Run it as a plain non-root user, or let the script drop privileges itself via the PASS 2 root-wrap path. Wrapping externally with `sudo` defeats the `runuser` handoff and re-entangles the audit with root context.

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
