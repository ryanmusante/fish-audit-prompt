fish-audit-prompt changelog

2026-04-19  Ryan Musante

- Tagged as v4.0.0
- 357 checks (346 static + 11 runtime). Sync with ry-install v4.1.3.
- remove AUDIT INFRA LESSONS section (4 items retired; lessons now folded into inline rg pattern comments).
- retire Phase 11 checks 168–170 (backup/rollback/dry-run): backup system removed in ry-install v3.5.0, --dry-run removed in v4.1.0, do_rollback never reintroduced. Phase 11 count 14→11.
- retire Phase 15 checks 227–230 (completions + test-all): --completions and --test-all modes removed in ry-install v4.1.0. Phase 15 count 19→19 (4 retired, 4 added: 227 flag coverage + dispatch consistency, 228 exit-code help-text enumeration, 232/233 profile-system invariants).
- reduce Phase 12 runtime matrix 15→11. Remove probes for: --lint (removed v3.48.20), --test-all (v4.1.0), --completions (v4.1.0), --logs (removed across v3.5.0–v3.7.13), --diff/--fix (v4.1.0), --dry-run/--all (v4.1.0), --force. Add: argparse --exclusive enforcement test (244), deprecated-flag rejection matrix (246), --check exit code 3/10 differentiation (245), SIGPIPE 141 + signal code 129/130/131/143 help-text check (249). Runtime checks renumbered 239–249.
- add Phase 14 subsection 14.25 v3.47.0–v3.51.15 CHANGES (9 checks, 334–342): --lint removal, _get_boot_time inlined + _progress_skip merged + _ry_count_managed_cases replaced, source-safety subsystem (_ry_exit + _RY_INSTALL_BAILING/SOURCED/LAST_EXIT), _ry_namespace_cleanup, BOOT_WIPE_MARKER ack gate, RY_RUN_TIMEOUT env, TIMESTAMP+$fish_pid race fix, profile system (v3.50.0) with _load_profile + _validate_profile + 26 required globals + trust model, flock -n -E 5 reclaim.
- add Phase 14 subsection 14.26 v4.0.0 CONFIG OVERHAUL (8 checks, 343–350): KERNEL_PARAMS 12→15, SYSCTL_VALUES 21→19, PKGS_ADD 15→14 + EXPECTED_SERVICES 3→4, nftables firewall baseline, systemd-coredump.socket masked, irqbalance removed from MASK, RADV env rework + drirc Mesa ≥22.3, RESOLVED_MDNS=resolve.
- add Phase 14 subsection 14.27 v4.0.1–v4.0.4 HARDENING (6 checks, 351–356): _install_fstab_opts chmod/chown --reference, log subdirs umask 0077, _ry_do_install_file post-deploy branches for 4 new dest types, 11 fish -c --preserve-status wrappers, _ry_verify_static terminal '*' + per-pid wait timeout sentinels, hash-pipeline pipestatus guard at 8 sites.
- add Phase 14 subsection 14.28 v4.1.0–v4.1.3 MODE SURFACE + REGRESSIONS (11 checks, 357–367): --test-all removal regression, --completions removal regression, signal handler SIG-prefix case matching (129/130/131/143), _install_preflight sudo-tag regex anchoring, SYU gate via RY_INSTALL_CONFIRM_SYSTEM_UPGRADE, MANIFEST_WRITE_FAILED decoupling, sdboot-manage update EXIT_BOOT_CRIT symmetry, flock dep SOFT, _tmpfile_key 8-char sha256 prefix at 7 sites, argparse --exclusive dispatcher rewrite + deprecated-flag handler, _write_footer drop `finished` + implicit_svcs derivation + _TRACKED_TMPFILES pruning + BLS exact-segment match + 6-site exit triple-form.
- update Phase 6 check 73 (cached _ROOT_UUID), 74 (KERNEL_PARAMS 15 entries), 76 (BOOT_WIPE ack gate), 99 (priority-99 header + string split -m1), 100 (SYSCTL_VALUES 19 entries with net-new + removed keys enumerated).
- update Phase 7 checks 103–106 (managed file count 15→16, _RY_MANAGED_FILE_COUNT rename, 6-step PROGRESS_STEPS).
- update PASS 1 STEP 1 GROUP D data collection: add ~45 new rg patterns covering v3.47.0–v4.1.3 deltas. Replace _ry_do_test_all and _ry_do_completions extractors with 0-hit regression checks. Update file-mapping comment "P14 v3.1.0-v3.46.0 checks" → "P14 v3.1.0-v4.1.3 checks".
- rewrite PASS 2 STEP 1 data collection: collapse Groups A/B/C/D/E into A (stdout/stderr sep) / B (4-subgroup exit-code matrix: help-version, verify/check/install-file, deprecated flags, exclusive violations) / C (SIGPIPE + redirect bug + help-text 141) / D (NO_COLOR enforcement on read-only modes only; write modes need sudo+profile). Drop dry-run FS snapshot (no --dry-run mode). Drop --lint stderr capture. Drop --test-all extended-timeout run.
- rewrite PASS 2 STEP 2 analysis: drop stale [DRY]/lint/test-all/completions assertions. Add deprecated-flags-all-return-2 assertion and exclusive-violations-all-return-2 assertion via grep on exit-codes.txt.
- rewrite PASS 2 STEP 3 checklist 239–249: single-source reference for stdout/stderr separation, exit code matrix, argparse --exclusive, deprecated flag handler, --check EXIT_PREFLIGHT vs EXIT_DRIFT differentiation, NO_COLOR, redirect-bug regression, SIGPIPE + signal help-text.
- update PASS 3 SUMMARY TABLE: P11 14→11, P12 15→11, P14 124→158, P15 19→19, TOTAL 330→357. Reorder "Runtime only" count 15→11. Expand SPECIAL HANDLING "Expect 0 hits" list with checks 334 (--lint), 335 (_get_boot_time/_progress_skip/_ry_count_managed_cases), 348 (irqbalance), 357 (--test-all), 358 (--completions), 367 (redundant footer field). Retire "WARN not exit 2" row and "Timeout exit" row (both referenced removed-mode behaviors). Retire "--test-all exit 1 on non-target" examine-stderr note; replace with --verify-static/runtime equivalent and --check 3/10 note.
- update check-range NOTE: P1-11 #1-129,133-167 (was 133-170), P14 #171-214,254-367 (was 254-333). Add retire note for 168-170 and addition note for 334-367.
- update header: Version 3.9.2→4.0.0, "Derived from: ry-install.fish v3.46.0 audit (2026-04-06)" → "v4.1.3 audit (2026-04-19)", Checks 330→357, Phases 15→16 structure descriptor (11 static + 1 runtime + 1 gap + 1 version-specifics + 1 supplemental + 1 profile-system).

2026-04-06  Ryan Musante

- Tagged as v3.9.2
- Audit fixes from v3.9.1 line-by-line review. No check additions/removals; total stays 330.
- fix L629/632/633/635/637/638/645/647: 8 Phase-14 rg patterns escaped `\|` inside single quotes, matching literal pipe instead of alternation. All 8 detection patterns silently returned 0 hits regardless of source content (V3100 manifest/ipv6/ssid/tmpdir/zram/lvm + V3460 noatime/coredump).
- fix L1141 NOTE: P14 check range `254-324` → `254-333` (9 checks 325-333 added in v3.9.0 but range note not propagated).
- fix L953 + L1024: file-count contradiction. SYSTEM=11 vs 12 vs 17 across L953/check103/check180. Reconciled to SYSTEM=12, USER=3, SERVICE=1 = 16; check 103 updated 11→12.
- fix L274 BASHISMS rg pattern: added `\[\[` and `\$\{` to alternation. Phase 1 check 1 explicitly lists both as bashisms; pattern previously omitted them (false negative).
- fix STEP 4 PACKAGE: zip command guarded against missing fixes-spec (cp guard from v3.9.1 was incomplete; zip arglist was unguarded).
- fix STEP 4 placeholders: `S=script; V=x.y.z` → `: ${S:?...} ${V:?...}` required-parameter expansion to prevent verbatim execution producing literal `audit-script-x.y.z.zip`.

2026-04-06  Ryan Musante

- Tagged as v3.9.1
- Mechanical staleness + clarity sweep. No check additions/removals; total stays 330 (315 static + 15 runtime).
- fix L94/L103 static-checklist count 306→315 (post-v3.9.0 phase 14 expansion not propagated to SETUP STEP 3).
- fix PASS 3 SUMMARY TABLE: phase 14 row 115→124, TOTAL 321→330; reorder phase 12 row in numerical sequence; add PASS column (PASS 1 vs PASS 2).
- fix L6 phase descriptor: drop dangling "+4.x" suffix (no corresponding phase content; Fish 4.x checks live inside phase 14).
- expand SPECIAL HANDLING: inline labels for check 137 (false-positive doc), 247 (modifier-only flags), and the 17 expect-0-hit regression checks (83, 171, 193, 296 annotated; rest enumerated).
- expand PASS 1 STEP 2 report contract: require per-phase tally (P1..P15 minus P12) summing to 315.
- fix PASS 3 STEP 4 PACKAGE: guard `cp` with `[ -f "$f" ]` existence check; previously failed silently when no fixes spec was generated.
- clarify PASS 3 STEP 3 schema row: declare F-N field set as multi-line block, not pipe-delimited (matches actual rendered instances).
- document UNIQUENESS VERIFICATION block as bash-only; depends on PASS 1 STEP 2 shims; do not fish-ify.
- promote AUDIT INFRA LESSONS from buried PURPOSE child to top-level section.
- annotate SETUP STEP 1 with explicit Debian/Ubuntu host assumption and rg pin re-verification reminder.

2026-04-06  Ryan Musante

- Tagged as v3.9.0
- 330 checks (315 static + 15 runtime). Sync with ry-install v3.46.0.
- add 9 checks (325–333): preempt=full param, page_lock_unfairness removal, netdev_max_backlog value, somaxconn addition, sysctl priority-99 header, string split -m1 sysctl, noatime dedup, usbhid.mousepoll removal, coredump.conf.d semantic verify.
- add 10 GROUP D rg patterns for v3.45–v3.46 changes.
- update Phase 14 header v3.44.0→v3.46.0 (115→124 checks).
- update subsection 14.24 v3.44.0→v3.46.0 (20→29 checks).

2026-04-05  Ryan Musante

- Tagged as v3.8.1
- 321 checks (306 static + 15 runtime). Sync with ry-install v3.44.0.
- add 20 checks (305–324): fstab, sysctl, irqbalance, service removals, sudo -n, IPv6, SSID, grep separators, TMPDIR guard, string collect, step timing, parallel stderr, LVM mask, validation stderr, quoted destinations.
- update 8 checks: amdgpu-performance, coredump.conf.d, file counts, scope counts, do_logs removal.
- add 17 v3.10–v3.44 pattern searches to GROUP D.
- remove KEY LESSONS section, trim INFRA LESSONS 18→4.
- rename 42 check descriptions for _ry_ prefix.
- fix static checklist count 286→306, total 301→321.
- Tagged as v3.8.0
- 301 checks. +7 Fish 4.0–4.6 compatibility checks (298–304).
- add 7 Fish 4.x pattern searches to GROUP D.
- pin rg >= 14.1.1 (false-negative bug #2884).
- add EVIDENCE RULE, set -uo pipefail, LC_ALL=C.UTF-8, python3 -ISc.
- add PATH sanitization, umask 077, trap EXIT/INT/TERM.
- replace SIZE GATE head -500 with skeleton extraction.
- fix checklist count 278→286, Phase 14 87→95.

2026-03-18  Ryan Musante

- Tagged as v3.7.32
- 294 checks. +1: sudo -n credential safety in _find_pacnew_files.
- Tagged as v3.7.31
- 293 checks. Deduplicate _py_extract calls; add VERSION guard.
- Tagged as v3.7.30
- 293 checks. Fix grep -c double-output; remove PCRE2 flag.

2026-03-17  Ryan Musante

- Tagged as v3.7.29
- 293 checks. 5 fixes: grep fallback, stdout modes, glob, budget, timeout.
- Tagged as v3.7.28
- 293 checks. Sync CHANGELOG with README; add PASS2 checks.
- Tagged as v3.7.27
- 293 checks. Expand dry-run FS monitoring; remove GROUP E dupes.
- Tagged as v3.7.26
- 293 checks. 6 fixes: SIZE GATE, dry-run FS, rg fallback, timeout, sort, labels.
- Tagged as v3.7.25
- 293 checks. 4 fixes: data mapping, GROUP_EXIT, severity, dedup.
- Tagged as v3.7.24
- 293 checks. Fix empty alternation in BASH AND-OR rg pattern.
- Tagged as v3.7.23
- 293 checks. +8 checks (289–296): fish -c injection, brace expansion, parallel status, glob, wait guard, worker cap, symlink, deprecated test operators.

2026-03-16  Ryan Musante

- Tagged as v3.7.13
- 285 checks. Unified version scheme (31.x → 3.7.x). 12 rg pattern fixes.
- Prior: v25 (254 checks) → v31.2.1 (285 checks), 22 releases.
