fish-audit-prompt changelog

2026-04-21  Ryan Musante

- v4.4.4 — 374 checks. README ↔ prompt cross-check sync. Check 79 hook order rewritten with the full 11-hook chain `base → systemd → autodetect → microcode → modconf → kms → keyboard → sd-vconsole → block → filesystems → fsck` (was a 6-hook excerpt missing autodetect/microcode/modconf/kms/block); regression note added re: absence of `resume` (sleep/hibernate masked). Checks 112 + 227 disambiguated — the README "Deprecated flags — DO NOT re-introduce" header at L246 is env-var-scoped (DXVK_ASYNC, DXVK_FRAME_RATE, WINE_FULLSCREEN_FSR; VKD3D_FRAME_RATE retained), NOT CLI flags; deprecated CLI flags (--all, --dry-run, --diff, --fix, --lint, --test-all, --completions, --logs) rejected only via `_early_usage_exit`. No ry-install delta.
- v4.4.3 — 374 checks. Fix 5 stale/broken regexes against ry-install v4.1.8. Check 325: `preempt=full` retired from KERNEL_PARAMS by v4.0.x; check rewritten as runtime dmesg-only verification (`Dynamic Preempt: full` → ≥1 hit). Check 356: bash-array `_ps[1]=0` syntax replaced with fish-native `test $_ps[1] -eq 0; and test $_ps[2] -eq 0` (→ 7 hits) + `-ne 0; or` failure form (→ 3 hits); stale `e3b0c44` hex literal removed; GROUP D V403 collection regex updated. Check 350: literal `MulticastDNS=resolve` → interpolation-chain check (`set -g RESOLVED_MDNS resolve` → 1 + `MulticastDNS=$RESOLVED_MDNS` → ≥2). Check 328 marked retired (somaxconn removed v4.0.0; supersedes-target check 344). Check 307 marked retired (irqbalance off MASK v4.0.0; supersedes-target check 348). SPECIAL HANDLING list extended (307, 325, 328). No ry-install delta.
- v4.4.2 — 374 checks. Fix 2 stale claims. Check 280 PROGRESS_STEPS rewritten for the v4.0.0 6-phase model (Preflight → Packages → Configuration → Services → Boot → Finalize); was a retired 20-step model with stale "entries 15-17"; verified 6 `_progress <phase>` calls + 1 Finalize-skip fast-path variant. Check 296 + GROUP D regex `test .* -o |test .* -a ` tightened to `test [^;]+ -[oa] [^;]+` — eliminates 4 false positives on `set --append` lines while preserving detection of genuine `test EXPR -[oa] EXPR`. No ry-install delta.
- v4.4.1 — 374 checks. Fix 5 corrections. Check 180 SYSCTL.D INTEGRATION: `MANAGED_FILE_COUNT=15` → `_RY_MANAGED_FILE_COUNT=16`, filename `99-ry-sysctl.conf` → `99-cachyos-sysctl.conf`, position `(12th)` → `(11th)`. Check 379/369: 'Profile destination key collision' label retained as v4.1.5 prefix (1 hit expected, not 0). Check 384: side-note attribution corrected — L5951 is top-level log-rotation, not `_acquire_lock` (which spans L377-452). PASS 2 STEP 1: root-rejection guard added (auto-detect, PASS2_FISH_USER override defaulting to `nobody`, runuser wrap, mktemp HOME with EXIT-trap cleanup) — closes false-fail when audit runs as root since ry-install exits 2 for `(id -u)==0`. Check 249 SIGPIPE: relaxed to `{0, 141}` — help (~1.6 KB) fits 64 KB pipe buffer so PIPESTATUS=0 is normal; static checks 189 + 321 remain authoritative. No ry-install delta.
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
