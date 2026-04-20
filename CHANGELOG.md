fish-audit-prompt changelog

2026-04-19  Ryan Musante

- Tagged as v4.3.0
- 370 checks (359 static + 11 runtime). Sync with ry-install v4.1.6.
- add Phase 14 subsection 14.31 v4.1.6 SIGNAL-SAFE JSONL REDIRECTS (1 check, 380): all 6 JSONL append writers (`_write_footer` L316, `_json_str` L1356, `_log` L1408, `_msg` L1425, `_emit_step_time` L1577, top-level header L5922) share the `>>"$LOG_FILE" 2>/dev/null` pattern. Closes stderr-noise window under signal-triggered log rotation where `rm` races the TOCTOU between `_log`'s `test -f "$LOG_FILE"; or return 0` entry guard and the subsequent append. Primary check expects 6 hits of the paired pattern; regression sub-check expects 0 bare `>>"$LOG_FILE"$` remaining.
- GROUP D data collection: +5 rg patterns for v4.1.6 deltas (V416 JSONL REDIRECT SITES, V416 JSONL REDIRECT COUNT, V416 BARE JSONL APPEND, V416 LOG GUARD TOCTOU, V416 JSONL WRITER FUNCTIONS).
- counter baseline refresh (comprehensive source check): stale "v4.1.4 baseline" labels in checks 197 (`command` prefix), 198 (`--argument-names`), 199 + 204 + 213 (scope counters) did not match ry-install v4.1.5/v4.1.6 ground truth. Re-measured against v4.1.6 source and relabeled as "v4.1.6 baseline": `command` prefix ~173 (was ~162), `set -l` ~590 (was ~378), bare `set` ~192 (was ~144), in-function `set -g` ~130 (was ~113), `_run` wraps ~56 (was ~58; within drift), `--argument-names` 21 (unchanged). Drift-indicator framing preserved — these are trend signals, not pass gates.
- PASS 3 SUMMARY TABLE: P14 170→171, TOTAL 369→370. Checklist sum-to target 358→359.
- PASS 3 SPECIAL HANDLING: add 380 (bare `>>"$LOG_FILE"` regression, primary expects 6 hits / sub-check expects 0) to "Expect 0 hits" list.
- check-range NOTE: P14 range #171-214,254-379 → #171-214,254-380; new line `Check 380 added in v4.3.0 audit prompt covering ry-install v4.1.6`.
- header: 4.2.0→4.3.0, source v4.1.5→v4.1.6, 369→370.
- update all v4.1.5 pins to v4.1.6: `Derived from` header, Phase 14 subtitle, GROUP D comment header, v3.47.0–v4.1.5 subgroup header, size-gate case label, data-file mapping comment, p14 missing-data warning, per-phase sum-to target, README Metrics/Phase/Files entries.
- ry-install not modified — audit prompt references v4.1.6 source only.

2026-04-19  Ryan Musante

- Tagged as v4.2.0
- 369 checks (358 static + 11 runtime). Sync with ry-install v4.1.5.
- add Phase 14 subsection 14.30 v4.1.5 DIAGNOSTICS REFINEMENT (1 check, 379): `_validate_profile` destination guard split into duplicate-source check + key-collision check. Verifies both new `_err` messages (`"Profile destination duplicated: '$_d'"` and `"'$_d' and '$_owner' both map to tmpfile key '$_k'"`) and 0-hit regression on retired v4.1.4 message (`"Profile destination key collision"`).
- rewrite check 369 to cross-reference check 379; regression pattern expanded from single legacy message to the two split diagnostics; legacy message added to "Expect 0 hits" list in PASS 3 SPECIAL HANDLING.
- GROUP D data collection: +4 rg patterns for v4.1.5 deltas (PROFILE GUARD SPLIT, PROFILE DUP SOURCE, PROFILE KEY COLLISION, PROFILE KEY RETIRED MSG). V414 VALIDATE PROFILE COLLISION GUARD pattern expanded to include both new messages.
- fix phase count header: `16 (11 static + 1 runtime + 1 gap analysis + 1 v3.1.0–v4.1.4 + 1 supplemental + 1 profile system)` → `15 (P1-P11 static · P12 runtime · P13 gap · P14 v3.1.0–v4.1.5 specifics · P15 supplemental)`. Profile system is Phase 15 subsection 15.13, not a separate phase.
- update all v4.1.4 pins to v4.1.5: `Derived from` header, Phase 14 subtitle, GROUP D comment header, `# ── v3.47.0–v4.1.5 additions ──`, `# ── v4.1.5 additions ──` subgroup, size-gate case label, data-file mapping comment, p14 missing-data warning.
- PASS 3 SUMMARY TABLE: P14 169→170, TOTAL 368→369. Checklist sum-to target 357→358.
- check-range NOTE: P14 range #171-214,254-378 → #171-214,254-379; new line `Check 379 added in v4.2.0 audit prompt covering ry-install v4.1.5`.
- header: 4.1.0→4.2.0, source v4.1.4→v4.1.5, 368→369.
- prose strip (user request): PURPOSE block collapsed to single `Scope` + `Policy` lines; SETUP STEP 1 "HOST:" lead-in folded into header; SETUP STEP 2 "Requires /bin/bash" prose folded into header; SETUP STEP 3 "EXTRACTION METHOD" paragraph trimmed; EXECUTION RULES / TOOL MANDATES / CONTEXT BUDGET / STALL PREVENTION lead-ins tightened; PASS 1 STEP 2 ANALYZE lead-in trimmed. Every check, command, regex, and code block preserved verbatim.

2026-04-19  Ryan Musante

- Tagged as v4.1.0
- 368 checks (357 static + 11 runtime). Sync with ry-install v4.1.4.
- add Phase 14 subsection 14.29 v4.1.4 PARITY + HARDENING (11 checks, 368–378): _tmpfile_key sha256 prefix retired, _validate_profile tmpfile-key collision guard, _ry_validate_configs merge loop simplified, _ry_verify_static hash-worker collector simplified, Job 2 empty svc_dsts guard, _kill_sudo_keepalive pkill descendant reap, _detect_lvm timeout 5→10, curl --connect-timeout on network + news-feed probes, _run stderr fish-native trim, boot-time LC_ALL=C parse.
- rewrite checks 355 (phantom sentinel capture dropped), 365 (sha256 prefix reverted to slash→underscore; collision guard moved to _validate_profile), 200 (START/END marker format updated; 4 START / 9 END).
- stale-counter sweep (185, 195–199, 201, 204, 212, 213): rebaseline structural counters and function-example lists against v4.1.4 source. Function count 80→76 (100% --description), 17 ── markers → 3 uppercase-capsule, ~686/~313/~85 → ~378/~144/~58 framed as drift indicators, remove retired functions from example lists.
- GROUP D data collection: +13 rg patterns for v4.1.4 deltas.
- PASS 3 SUMMARY TABLE: P14 158→169, TOTAL 357→368. Expand "Expect 0 hits" with 368, 370, 371, 377.
- check-range NOTE: P14 #171-214,254-378; addition note for 368-378.
- header: 4.0.0→4.1.0, source v4.1.3→v4.1.4, 357→368.
- trim: collapse multi-line `#` comment blocks in shell snippets to single lines (rg pin, GROUP E header, post-wait guard, NON-TARGET HARDWARE prose).

2026-04-19  Ryan Musante

- Tagged as v4.0.0
- 357 checks (346 static + 11 runtime). Sync with ry-install v4.1.3.
- retire checks 168–170 (backup/rollback/--dry-run: features removed v3.5.0–v4.1.0); retire 227–230 (completions + test-all: modes removed v4.1.0) and reuse slots for flag coverage + exit-code help-text + profile-system invariants.
- reduce Phase 12 runtime matrix 15→11: drop probes for removed modes; add argparse --exclusive enforcement, deprecated-flag rejection matrix, --check 3/10 differentiation, SIGPIPE + signal help-text. Renumber 239–249.
- add Phase 14 subsections 14.25–14.28 covering v3.47.0–v4.1.3 (34 checks, 334–367): --lint removal, source-safety subsystem, profile system, BOOT_WIPE ack, RY_RUN_TIMEOUT, TIMESTAMP+$fish_pid, flock reclaim, KERNEL_PARAMS/SYSCTL_VALUES/PKGS overhaul, nftables baseline, systemd-coredump.socket masked, RADV env rework, RESOLVED_MDNS=resolve, _install_fstab_opts chmod/chown --reference, --preserve-status wrappers, hash pipestatus guard, --test-all/--completions removal, SIG-prefix signal case matching, SYU ack gate, MANIFEST_WRITE_FAILED decoupling, sdboot-manage EXIT_BOOT_CRIT, flock SOFT, _tmpfile_key, argparse --exclusive dispatcher, _write_footer finished drop + implicit_svcs derivation + BLS exact-segment + 6-site exit triple-form.
- update Phase 6 checks 73–76, 99–100 (cached _ROOT_UUID, KERNEL_PARAMS 15, BOOT_WIPE ack, priority-99 header, SYSCTL_VALUES 19).
- update Phase 7 checks 103–106 (managed file count 16, _RY_MANAGED_FILE_COUNT rename, 6-step PROGRESS_STEPS).
- GROUP D: +45 rg patterns for v3.47.0–v4.1.3.
- rewrite PASS 2: collapse groups A–E into 4 subgroups; drop --dry-run/--lint/--test-all captures; add deprecated-flags + exclusive-violations assertions.
- header: 3.9.2→4.0.0, source v3.46.0→v4.1.3, 330→357.

2026-04-06  Ryan Musante

- v3.9.2 — line-by-line audit fixes. No check count change (stays 330). Fix 8 Phase-14 rg patterns with escaped `\|` in single quotes (silent false negatives); file-count reconciliation SYSTEM=12/USER=3/SERVICE=1=16; P14 check range 254-324→254-333; BASHISMS rg pattern completion; STEP 4 zip + placeholder guards.
- v3.9.1 — staleness + clarity sweep. No check count change. Counter fixes (306→315, 321→330, phase 14 115→124); PASS 3 table reorder + PASS column; SPECIAL HANDLING expansion; per-phase tally requirement; UNIQUENESS VERIFICATION documented bash-only.
- v3.9.0 — 330 checks (315 static + 15 runtime). Sync with ry-install v3.46.0. +9 checks (325–333): preempt=full, page_lock_unfairness removal, netdev_max_backlog, somaxconn, sysctl priority-99 header, string split -m1, noatime dedup, usbhid.mousepoll removal, coredump.conf.d semantic verify.

2026-04-05  Ryan Musante

- v3.8.1 — 321 checks. Sync with ry-install v3.44.0. +20 checks (305–324) covering fstab, sysctl, service removals, sudo -n, IPv6, SSID, LVM mask, quoted destinations. +17 GROUP D patterns.
- v3.8.0 — 301 checks. +7 Fish 4.0–4.6 compatibility checks (298–304). Pin rg ≥14.1.1 (bug #2884). Add EVIDENCE RULE, set -uo pipefail, LC_ALL=C.UTF-8, PATH sanitization, umask 077.

2026-03-18  Ryan Musante

- v3.7.32 — 294 checks. +1: sudo -n credential safety in _find_pacnew_files.
- v3.7.31 — 293. Deduplicate _py_extract calls; VERSION guard.
- v3.7.30 — 293. Fix grep -c double-output; remove PCRE2 flag.

2026-03-17  Ryan Musante

- v3.7.29–v3.7.23 — 293 checks. Accumulated fixes: grep fallback, stdout modes, glob handling, budget + timeout tuning, SIZE GATE, dry-run FS monitoring, GROUP E dedup, data mapping, GROUP_EXIT, severity labels, BASH AND-OR rg pattern. v3.7.23 +8 checks (289–296): fish -c injection, brace expansion, parallel status, glob, wait guard, worker cap, symlink, deprecated test operators.

2026-03-16  Ryan Musante

- v3.7.13 — 285 checks. Unified version scheme (31.x → 3.7.x). 12 rg pattern fixes.
- Prior lineage: v25 (254 checks) → v31.2.1 (285 checks), 22 releases.
