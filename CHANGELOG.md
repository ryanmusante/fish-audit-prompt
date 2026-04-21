fish-audit-prompt changelog

2026-04-20  Ryan Musante

- v4.4.1 — 374 checks (363 static + 11 runtime). Self-audit fixes; no ry-install delta (audits v4.1.8 unchanged). 5 corrections verified against ry-install source: (1) check 180 SYSCTL.D INTEGRATION — stale `MANAGED_FILE_COUNT=15`, stale filename `99-ry-sysctl.conf`, and stale position `(12th)` all corrected to `_RY_MANAGED_FILE_COUNT=16`, `99-cachyos-sysctl.conf`, `(11th entry; 12th is 99-nvme-rqaffinity.rules added v3.51.x)` per actual SYSTEM_DESTINATIONS array at ry-install L598-611 and `_RY_MANAGED_FILE_COUNT 16` at L161; (2) check 379 + check 369 `Profile destination key collision → 0 hits` false-positive — v4.1.5 split kept the label as prefix of the narrower key-collision branch (ry-install L929); assertion reframed to `1 hit` with scope-change note; GROUP D data-collection label renamed from `V415 PROFILE KEY RETIRED MSG` to `V415 PROFILE KEY COLLISION LABEL (retained as prefix; expect 1)`; SPECIAL HANDLING entry for 379 updated accordingly; (3) check 384 side-note `_acquire_lock (L5951)` misattribution corrected — `_acquire_lock` spans L377-452 only; L5951 `-printf '%T@\t%p\0'` is in the top-level log-rotation block post function declarations; (4) PASS 2 STEP 1 pre-flight lacked root-rejection guard — ry-install exits 2 (EXIT_USAGE) for `(id -u)==0` at L66-68, false-failing every PASS 2 test in root containers; added auto-detect root-guard with PASS2_FISH_USER env override (default `nobody`), runuser(1) availability check, target-user existence check, script-readability check, and `mktemp -d` + `chmod 0777` writable HOME creation with EXIT-trap cleanup (system users default to `HOME=/nonexistent` which breaks ry-install log directory creation); clean skip with actionable [CRITICAL] message if any guard fails; 13 PASS 2 fish invocations wrapped with `$FISH_PREFIX` (empty when non-root, `runuser -u $PASS2_USER -- env HOME=$PASS2_HOME` when root); SIGPIPE `/bin/bash -c` subshell receives `$FISH_PREFIX` as positional arg `$4` since single-quoted bash -c does not expand parent env; NO_COLOR=1 line repositioned to `env NO_COLOR=1` inside runuser boundary for env propagation; live end-to-end tested as root → `--help` exit 0 / stdout 1587 / stderr 0, `--version` exit 0 / stdout `v4.1.8`, `--bogus` exit 2, `--all` (deprecated) exit 2, NO_COLOR=1 0 ANSI escapes, `--verify-static --check` exit 2; (5) check 249 SIGPIPE `PIPESTATUS[0]=141` expectation unreliable — ry-install `--help` is ~1.6 KB, well below 64 KB Linux default pipe buffer, so fish flushes entire help before head closes and PIPESTATUS=0 is the normal outcome; relaxed to accept `{0, 141}` with strict verification delegated to static checks 189 (_cleanup_pipe --on-signal PIPE) + 321 (SIGPIPE HANDLER rg pattern); help text exit-codes list in check text extended to include 141. Fix 3 tightened `_acquire_lock` end-line from `~L460` to exact `L377-452`. Fix 4a exit-code corrected in two places from `exit 1` to `exit 2 (EXIT_USAGE)` per ry-install L23 `set -g EXIT_USAGE 2` and L68 `_ry_exit $EXIT_USAGE`. No check-count delta (363 static + 11 runtime); no counter rebaseline.

2026-04-20  Ryan Musante

- v4.4.0 — 374 checks (363 static + 11 runtime). Sync ry-install v4.1.8. +4 (381–384): Phase 14.32 HYGIENE + HARDENING — `_install_packages` pipeline-phase comments normalized (sync _info dropped, phase-4 cross-reference); `_ry_show_help` `--` description reflects dispatcher positional rejection + NO_COLOR added to ENVIRONMENT block; `_install_fstab_opts` deregisters intermediate `$tmpfstab` from `_TRACKED_TMPFILES` after atomic rename; `_preflight_boot_sanity` 3 find enumerations (vmlinuz/initramfs/loader-entries) switched to `-print0 | string split0` for `\n`-in-filename safety. +14 GROUP D rg patterns (V418 *). v4.1.7 (README/CHANGELOG-only) carries no auditable script delta. PASS 3: P14 171→175, TOTAL 370→374. Checklist-save gate 359→363. No counter rebaseline needed — v4.1.8 deltas do not affect set -l / set -g / bare-set totals.

2026-04-19  Ryan Musante

- v4.3.0 — 370 checks (359 static + 11 runtime). Sync ry-install v4.1.6. +1 (380): Phase 14.31 SIGNAL-SAFE JSONL REDIRECTS — all 6 JSONL append writers paired `>>"$LOG_FILE" 2>/dev/null`; closes stderr-noise window under signal-triggered log rotation. +5 GROUP D rg patterns (V416 *). Counter rebaseline 197–199, 204, 213 against v4.1.6. PASS 3: P14 170→171, TOTAL 369→370.
- v4.2.0 — 369 checks (358 static + 11 runtime). Sync ry-install v4.1.5. +1 (379): Phase 14.30 DIAGNOSTICS REFINEMENT — `_validate_profile` destination guard split into duplicate-source + key-collision checks. Rewrite 369; legacy message added to "Expect 0 hits". +4 GROUP D rg patterns. Phase count header corrected 16→15 (profile system is 15.13, not separate). PASS 3: P14 169→170, TOTAL 368→369. Prose strip across PURPOSE/SETUP/EXECUTION RULES; every check, command, regex preserved.
- v4.1.0 — 368 checks (357 static + 11 runtime). Sync ry-install v4.1.4. +11 (368–378): Phase 14.29 PARITY + HARDENING — _tmpfile_key sha256 prefix retired, _validate_profile collision guard, merge/hash-worker simplified, Job 2 svc_dsts guard, pkill descendant reap, _detect_lvm 5→10s, curl --connect-timeout, _run stderr trim, boot-time LC_ALL=C. Rewrite 200, 355, 365. Stale-counter sweep (185, 195–199, 201, 204, 212, 213). +13 GROUP D rg patterns. PASS 3: P14 158→169, TOTAL 357→368.
- v4.0.0 — 357 checks (346 static + 11 runtime). Sync ry-install v4.1.3. Retire 168–170 (backup/rollback/--dry-run) and 227–230 (completions/test-all); slots reused for flag coverage, exit-code help-text, profile-system invariants. Phase 12 runtime 15→11 (drop removed-mode probes; add argparse --exclusive, deprecated-flag rejection, --check 3/10, SIGPIPE help-text). +34 (334–367): Phase 14.25–14.28 v3.47.0–v4.1.3 covering source-safety, profile system, BOOT_WIPE/SYU acks, kernel/sysctl/pkg overhaul, nftables, RADV, _tmpfile_key, argparse --exclusive dispatcher, _write_footer/BLS/exit-triple-form. Phase 6 (73–76, 99–100), Phase 7 (103–106) updated. +45 GROUP D rg patterns. PASS 2 collapsed A–E → 4 subgroups.

2026-04-06  Ryan Musante

- v3.9.2 — 330 checks. Fix 8 Phase-14 rg patterns with escaped `\|` in single quotes (silent false negatives); SYSTEM=12/USER=3/SERVICE=1=16 reconciliation; P14 range 254-324→254-333; BASHISMS rg completion; STEP 4 zip + placeholder guards.
- v3.9.1 — 330 checks. Counter fixes (306→315, 321→330, P14 115→124); PASS 3 table reorder + PASS column; SPECIAL HANDLING expansion; per-phase tally; UNIQUENESS VERIFICATION documented bash-only.
- v3.9.0 — 330 checks (315 static + 15 runtime). Sync ry-install v3.46.0. +9 (325–333): preempt=full, page_lock_unfairness removal, netdev_max_backlog, somaxconn, sysctl priority-99 header, string split -m1, noatime dedup, usbhid.mousepoll removal, coredump.conf.d semantic verify.

2026-04-05  Ryan Musante

- v3.8.1 — 321 checks. Sync ry-install v3.44.0. +20 (305–324): fstab, sysctl, service removals, sudo -n, IPv6, SSID, LVM mask, quoted destinations. +17 GROUP D rg patterns.
- v3.8.0 — 301 checks. +7 Fish 4.0–4.6 compat (298–304). Pin rg ≥ 14.1.1 (bug #2884). Add EVIDENCE RULE, set -uo pipefail, LC_ALL=C.UTF-8, PATH sanitization, umask 077.

2026-03-18  Ryan Musante

- v3.7.32 — 294 checks. +1: sudo -n credential safety in _find_pacnew_files.
- v3.7.31 — 293. Deduplicate _py_extract calls; VERSION guard.
- v3.7.30 — 293. Fix grep -c double-output; remove PCRE2 flag.

2026-03-17  Ryan Musante

- v3.7.29–v3.7.23 — 293 checks. Accumulated fixes: grep fallback, stdout modes, glob handling, budget + timeout tuning, SIZE GATE, dry-run FS monitoring, GROUP E dedup, data mapping, GROUP_EXIT, severity labels, BASH AND-OR rg pattern. v3.7.23 +8 (289–296): fish -c injection, brace expansion, parallel status, glob, wait guard, worker cap, symlink, deprecated test operators.

2026-03-16  Ryan Musante

- v3.7.13 — 285 checks. Unified version scheme (31.x → 3.7.x). 12 rg pattern fixes.
- Prior lineage: v25 (254 checks) → v31.2.1 (285 checks), 22 releases.
