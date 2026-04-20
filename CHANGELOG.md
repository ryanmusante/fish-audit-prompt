fish-audit-prompt changelog

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
