# fish-audit-prompt changelog

v4.5.0  2026-04-22
- Self-audit fix 1 CRIT + 3 HIGH + 7 MED + 10 LOW against v4.4.5. 21 of 25 findings landed; 4 deferred or unresearched.
- F-1 CRIT — PASS 2 STEP 1 (L2180–2181): `chmod -R 0777 "$PASS2_HOME"` → `chown -R "$PASS2_USER:" "$PASS2_HOME"; chmod -R 0700 "$PASS2_HOME"`. fish sources `~/.config/fish/config.fish` even non-interactively (verified fish 3.7.0); 0777 opened local-attacker race between mkdir (L2180) and first fish spawn (L2199) for arbitrary code execution as `$PASS2_USER` when audit runs as root. chown + 0700 closes the race with no write access for others.
- F-2 HIGH — SETUP STEP 2 (L44): `rg` fallback `rg() { grep -P --color=never "$@"; }` → hard-fail `exit 2`. `grep -P -F` emits "conflicting matchers specified" (verified Ubuntu 24.04 grep 3.11); PASS 3 uniqueness verification at L2465/L2477 passes `-cF` to the shim and every Before pattern falsely reports error, blocking spec generation. STEP 1 auto-installs rg; no graceful-degrade path remains.
- F-3 HIGH — SETUP STEP 1 (L19–22): apt-get-only installer → auto-detect `apt-get` / `pacman` / `dnf`. Header promised "adapt for other distros" but body was Debian/Ubuntu only; silent failure on Arch (CachyOS host) cascaded to shim degradation. `ripgrep` added to apt install list (previously external download).
- F-4 HIGH — SETUP STEP 2 (L56): `PATH='/usr/local/bin:/usr/bin:/bin'` → `+/usr/sbin:/sbin`. `runuser(1)` lives in `/usr/sbin` on Debian/Ubuntu (verified Ubuntu 24.04); PASS 2 root-wrap at L2163 ran `command -v runuser` under sanitized PATH → exit 127 → false "[CRITICAL] PASS 2 SKIPPED" on every Debian/Ubuntu root audit.
- F-5 MED — SETUP STEP 1 (L24): regex `14\.1\.[1-9]|14\.[2-9]|1[5-9]\.|[2-9][0-9]\.` → numeric-tuple compare via `IFS=. read -r _rg_maj _rg_min _rg_pat <<<"$_ver"`. Regex rejected 100+ major; tuple compare has no upper bound and has already required one ad-hoc extension.
- F-7 MED — PASS 3 STEP 1 (L2362–2369): fish-in-bash `for sev in CRITICAL...` + separate `TOTAL` tail-1 chain → single awk `-F'[][]'` one-pass. 7× rg + fish startup (~50–80 ms × 7) → O(n) awk single scan; also auto-fixes the `tail -1` antipattern at L2369.
- F-8 MED — PASS 1 STEP 1 (L729–742): nested `while read` EMPTY SECTION GUARD with per-header `grep -nF | head -n 1 | cut` + `awk -v start=` → single-pass awk per file. O(n²) → O(n); ~240 scans collapse to 1 on p14.txt.
- F-10 MED — SETUP STEP 2 (after L49): added `[ -n "$BASH_VERSION" ] || { ... exit 2; }`. Under dash, `export -f` at L49 hard-aborts the -c script (verified `dash -c 'echo before; export -f foo 2>/dev/null; echo after; true'` prints only "before", exit 2) — hardening lines L50–62 never execute without the guard.
- F-11 MED — PASS 2 STEP 1 (11 sites): `FISH_PREFIX="runuser -u $PASS2_USER -- env HOME=$PASS2_HOME"` (word-split) → `FISH_PREFIX_ARGS=(runuser -u "$PASS2_USER" -- env "HOME=$PASS2_HOME")` array. Every `$FISH_PREFIX` use → `"${FISH_PREFIX_ARGS[@]}"`. Space in `$PASS2_HOME` now tolerated; SIGPIPE test's former `/bin/bash -c` word-split workaround no longer needed.
- F-12 LOW — SETUP STEP 1 (L25–26): rg pin `14.1.1` → `15.0.1`. Fixes parent-dir gitignore bugs, large-gitignore memory regression, adds Jujutsu repo support; version-check regex at L24 already accepts 15.x.
- F-13 LOW — PASS 1 STEP 1 GROUP A1 (L189): bashism detector piped through `rg -v '^[0-9]+:\s*#|ExecStart=|^\s*awk |<<-?\s*['\''"]?EOF'`. Pre-filters ExecStart systemd-unit `&&`, awk-body `${...}`, heredoc contents that Phase 1 L782–784 already marks exempt — exemption was post-hoc; pre-filter reduces noise.
- F-14 LOW — PASS 2 STEP 1 (L2253–2257): `/bin/bash -c '...$FP fish...${PIPESTATUS[0]}...'` wrapper → direct `timeout "$HELP_TIMEOUT" "${FISH_PREFIX_ARGS[@]}" fish --help 2>&1 | head -n 1`. Wrapper existed solely to word-split `$FISH_PREFIX`; redundant after F-11. Test still catches non-{0,141} (e.g., fish crash on pipe close).
- F-15 LOW — PASS 1 STEP 1 (L698–727): serial SIZE GATE for-loop → per-file `( ... ) & done; wait`. 7 files × ~100ms each serial (~700ms) → parallel (~150ms; capped by nproc).
- F-16 LOW — PASS 1 STEP 1 (L745): `wc -l "$D"/*.txt` → `shopt -s nullglob; _files=("$D"/*.txt); shopt -u nullglob; [ ${#_files[@]} -gt 0 ] && wc -l "${_files[@]}" || echo "[INFO] no data files produced in $D"`. Default bash has nullglob off; empty glob passed literal `$D/*.txt` → wc error "No such file or directory" exit 1.
- F-17 LOW — PASS 2 STEP 1 (L2271–2275) + checklist 247: added second NO_COLOR test pass with `env NO_COLOR=` (empty string) + ANSI >0 assertion. no-color.org v1.0 (2022) explicitly requires "present AND not an empty string" — empty value must NOT suppress color. Check 247 rewritten as two-axis (suppress + not-suppress).
- F-18 LOW — GROUP D + GROUP C2 (L330, 372, 417, 427, 554): 5 `rg -nc` → `rg -c`. `-n` silently ignored under `-c`; violates user LINT rule.
- F-19 LOW — PASS 2 STEP 2 (L2290): `rg -oP 'VERSION "\K[^"]+' || rg -o ... | sed 's/VERSION "//'` → `rg -o 'VERSION "[^"]+"' | sed 's/.*VERSION "//; s/"$//' | head -n 1`. `\K` requires PCRE2; `cargo install ripgrep --no-default-features` produces rg without PCRE2, old fallback stripped only leading quote leaving trailing `"` → false MISMATCH.
- F-20 LOW — PASS 2 STEP 1 (L2182): `trap '...' EXIT` → `trap '...' EXIT INT TERM HUP`. Matches L185 outer cleanup trap pattern; guarantees tmpdir removal on Ctrl-C during 4-parallel fish spawn.
- F-21 LOW — PASS 2 STEP 1 (L2196 + help/version sites): `RUNTIME_TIMEOUT=30` uniform → split `HELP_TIMEOUT=${HELP_TIMEOUT:-5}` for `--help/-h/--version/-v` invocations. Measured `fish script.fish --help` = 0.023s warm; 30s was 1300× headroom; 5s still 200× while surfacing regressions 6× faster.
- F-24 LOW — Phase 14.1 check 171 + GROUP D (new rg pattern at L308): added sub-check 171b `KVER GATE ORDERING` verifying `_ry_check_kernel_version` definition line < first `$KVER_MAJOR` / `$KVER_MINOR` consumer line. Check 171 validated KVER top-level init + no inline `uname -r` but did not verify the kernel-version gate runs before consumers.
- F-25 LOW — PASS 1 STEP 1 (L684): `tail -1` → `tail -n 1`. Audit's own check 141 (L1081) flags `head -1`/`tail -1` as antipattern requiring `-n 1`. Second violation at L2369 auto-fixed by F-7.
- Comment-style — 5 multi-line `#` comment blocks collapsed to single-line form (project convention; matches ry-install v4.1.14 "Three multi-line `#` comment blocks collapsed to single-line form" delta).
- Count bumps — header version 4.4.5 → 4.5.0; total checks 374 → 375 (F-24 +1 sub-check); static checks 363 → 364; Phase 14 count 175 → 176; checklist-save gate 363 → 364; summary table P14 175 → 176 and TOTAL 374 → 375; README version 4.4.5 → 4.5.0 and check badge 374 → 375.
- README — added `awk` / `curl` / `zip` to Prerequisites table (F-9) with POSIX/any versions and hard-fail fallback semantics; added "Security Considerations" section (F-22) documenting SETUP STEP 2 environment hardening, bash-only contract, PASS 2 root-handling invariants, cleanup trap signal coverage, and invocation rules (never `sudo bash prompt.sh`). README also notes package installer auto-detect across apt-get/pacman/dnf.
- Drift note — header (L9) flags that ry-install v4.1.9–v4.1.14 deltas are NOT yet encoded: PROGRESS_STEPS array removal (invalidates checks 218, 280, GROUP C2 extractors at L278–279); SYSCTL_VALUES count is now 21 not 19 (check 100); ENV_VARS count is now 13 not 12; v4.1.10 added sudo -n parity in `_ry_verify_runtime`; v4.1.12 added null-delim boot-wipe marker enumeration. v4.6.0 will re-sync against ry-install v4.1.14 baseline.

Deferred
- F-6 MED — Extract 9 inline `python3 -ISc` heredocs to `helpers/*.py`. Significant structural change (adds `helpers/` subdir to packaging, updates 9 call sites, updates SETUP STEP 1). Deferred to v4.6.0 or v5.0.0 alongside the ry-install v4.1.14 baseline sync.

Unresearched (tracked for v4.6.0)
- `rg 13.x` single-file `rg -c` 0-match behavior — could not install 13.x under network allowlist to verify prior spec speculation (retracted).
- `set -l` block-scope vs function-scope in fish 4.x — scope-shadow detector at L458–498 may over-report; needs fish 3.4 / 4.0 / 4.6 cross-version test before adjusting detector.
- `bat --plain -r START:END` syntax stability across bat 0.24 → 0.26.
- `fish_indent --check` exit-code semantics across fish 3.7 → 4.6 — PASS 1 STEP 1 L190 depends on exit 0 == pass.
- `python3 -I` implied-flag set across Python 3.12/3.13 releases.
- `rg --generate man / complete-fish / complete-bash` (ripgrep 14.x+) — not leveraged; potential future improvement.

v4.4.5  2026-04-21
- 374 checks. Fix 2 HIGH + 1 LOW findings from v4.4.4 → v4.1.8 self-audit. F-1 PASS 2 STEP 1 HOME-race: 4 parallel fish invocations (--help, -h, --version, -v) shared one PASS2_HOME and raced to `mkdir -p HOME/.local/share/fish` + `HOME/.config/fish`, producing stderr "warning-path: The error was 'File exists'" (measured: help-err=257B, h-err=279B, ver-err=536B against fish 3.7.0) — false-failed checks 239/240 ("0 stderr bytes") on every root-wrapped run. Fix: pre-create both subdirs + `chmod -R 0777` before the parallel fork; verified 0 stderr across all 4.
- F-2 GROUP C2 PROGRESS DEFINED + PROGRESS CALLED extractors rewritten for ry-install v4.0.0+ bareword array (`Preflight Packages ...`) + bareword call sites (`_progress Preflight`); prior `rg -o '"[^"]*"'` / `rg '_progress "'` patterns returned 0 hits against v4.1.8, making checks 218 + 280 vacuous.
- F-3 counter baselines refreshed v4.1.6→v4.1.8 in checks 197, 198, 199, 204, 213: bare set 192→193 (3 sites), _run wraps ~56→60 (1 site), v4.1.6 baseline label → v4.1.8 (6 occurrences). GROUP D section label + SIZE GATE phase label + DATA FILE mapping + empty-guard INFO message updated from "v3.1.0–v4.1.6 specifics" → "v3.1.0–v4.1.8 specifics" (4 sites). Retained v4.1.6-specific refs in check 380 (historically correct — check added in v4.3.0 for v4.1.6 delta). No ry-install delta.

v4.4.4  2026-04-21
- 374 checks. README ↔ prompt cross-check sync. Check 79 hook order rewritten with the full 11-hook chain `base → systemd → autodetect → microcode → modconf → kms → keyboard → sd-vconsole → block → filesystems → fsck` (was a 6-hook excerpt missing autodetect/microcode/modconf/kms/block); regression note added re: absence of `resume` (sleep/hibernate masked). Checks 112 + 227 disambiguated — the README "Deprecated flags — DO NOT re-introduce" header at L246 is env-var-scoped (DXVK_ASYNC, DXVK_FRAME_RATE, WINE_FULLSCREEN_FSR; VKD3D_FRAME_RATE retained), NOT CLI flags; deprecated CLI flags (--all, --dry-run, --diff, --fix, --lint, --test-all, --completions, --logs) rejected only via `_early_usage_exit`. No ry-install delta.

v4.4.3  2026-04-21
- 374 checks. Fix 5 stale/broken regexes against ry-install v4.1.8. Check 325: `preempt=full` retired from KERNEL_PARAMS by v4.0.x; check rewritten as runtime dmesg-only verification (`Dynamic Preempt: full` → ≥1 hit). Check 356: bash-array `_ps[1]=0` syntax replaced with fish-native `test $_ps[1] -eq 0; and test $_ps[2] -eq 0` (→ 7 hits) + `-ne 0; or` failure form (→ 3 hits); stale `e3b0c44` hex literal removed; GROUP D V403 collection regex updated. Check 350: literal `MulticastDNS=resolve` → interpolation-chain check (`set -g RESOLVED_MDNS resolve` → 1 + `MulticastDNS=$RESOLVED_MDNS` → ≥2). Check 328 marked retired (somaxconn removed v4.0.0; supersedes-target check 344). Check 307 marked retired (irqbalance off MASK v4.0.0; supersedes-target check 348). SPECIAL HANDLING list extended (307, 325, 328). No ry-install delta.

v4.4.2  2026-04-21
- 374 checks. Fix 2 stale claims. Check 280 PROGRESS_STEPS rewritten for the v4.0.0 6-phase model (Preflight → Packages → Configuration → Services → Boot → Finalize); was a retired 20-step model with stale "entries 15-17"; verified 6 `_progress <phase>` calls + 1 Finalize-skip fast-path variant. Check 296 + GROUP D regex `test .* -o |test .* -a ` tightened to `test [^;]+ -[oa] [^;]+` — eliminates 4 false positives on `set --append` lines while preserving detection of genuine `test EXPR -[oa] EXPR`. No ry-install delta.

v4.4.1  2026-04-21
- 374 checks. Fix 5 corrections. Check 180 SYSCTL.D INTEGRATION: `MANAGED_FILE_COUNT=15` → `_RY_MANAGED_FILE_COUNT=16`, filename `99-ry-sysctl.conf` → `99-cachyos-sysctl.conf`, position `(12th)` → `(11th)`. Check 379/369: 'Profile destination key collision' label retained as v4.1.5 prefix (1 hit expected, not 0). Check 384: side-note attribution corrected — L5951 is top-level log-rotation, not `_acquire_lock` (which spans L377-452). PASS 2 STEP 1: root-rejection guard added (auto-detect, PASS2_FISH_USER override defaulting to `nobody`, runuser wrap, mktemp HOME with EXIT-trap cleanup) — closes false-fail when audit runs as root since ry-install exits 2 for `(id -u)==0`. Check 249 SIGPIPE: relaxed to `{0, 141}` — help (~1.6 KB) fits 64 KB pipe buffer so PIPESTATUS=0 is normal; static checks 189 + 321 remain authoritative. No ry-install delta.

v4.4.0  2026-04-21
- 374 checks (363 static + 11 runtime). Sync ry-install v4.1.8. +4 (381–384): Phase 14.32 HYGIENE + HARDENING — `_install_packages` pipeline-phase comments normalized (sync _info dropped, phase-4 cross-reference); `_ry_show_help` `--` description reflects dispatcher positional rejection + NO_COLOR added to ENVIRONMENT block; `_install_fstab_opts` deregisters intermediate `$tmpfstab` from `_TRACKED_TMPFILES` after atomic rename; `_preflight_boot_sanity` 3 find enumerations (vmlinuz/initramfs/loader-entries) switched to `-print0 | string split0` for `\n`-in-filename safety. +14 GROUP D rg patterns (V418 *). v4.1.7 (README/CHANGELOG-only) carries no auditable script delta. PASS 3: P14 171→175, TOTAL 370→374. Checklist-save gate 359→363. No counter rebaseline needed — v4.1.8 deltas do not affect set -l / set -g / bare-set totals.

v4.3.0  2026-04-19
- 370 checks (359 static + 11 runtime). Sync ry-install v4.1.6. +1 (380): Phase 14.31 SIGNAL-SAFE JSONL REDIRECTS — all 6 JSONL append writers paired `>>"$LOG_FILE" 2>/dev/null`; closes stderr-noise window under signal-triggered log rotation. +5 GROUP D rg patterns (V416 *). Counter rebaseline 197–199, 204, 213 against v4.1.6. PASS 3: P14 170→171, TOTAL 369→370.

v4.2.0  2026-04-19
- 369 checks (358 static + 11 runtime). Sync ry-install v4.1.5. +1 (379): Phase 14.30 DIAGNOSTICS REFINEMENT — `_validate_profile` destination guard split into duplicate-source + key-collision checks. Rewrite 369; legacy message added to "Expect 0 hits". +4 GROUP D rg patterns. Phase count header corrected 16→15 (profile system is 15.13, not separate). PASS 3: P14 169→170, TOTAL 368→369. Prose strip across PURPOSE/SETUP/EXECUTION RULES; every check, command, regex preserved.

v4.1.0  2026-04-19
- 368 checks (357 static + 11 runtime). Sync ry-install v4.1.4. +11 (368–378): Phase 14.29 PARITY + HARDENING — _tmpfile_key sha256 prefix retired, _validate_profile collision guard, merge/hash-worker simplified, Job 2 svc_dsts guard, pkill descendant reap, _detect_lvm 5→10s, curl --connect-timeout, _run stderr trim, boot-time LC_ALL=C. Rewrite 200, 355, 365. Stale-counter sweep (185, 195–199, 201, 204, 212, 213). +13 GROUP D rg patterns. PASS 3: P14 158→169, TOTAL 357→368.

v4.0.0  2026-04-19
- 357 checks (346 static + 11 runtime). Sync ry-install v4.1.3. Retire 168–170 (backup/rollback/--dry-run) and 227–230 (completions/test-all); slots reused for flag coverage, exit-code help-text, profile-system invariants. Phase 12 runtime 15→11 (drop removed-mode probes; add argparse --exclusive, deprecated-flag rejection, --check 3/10, SIGPIPE help-text). +34 (334–367): Phase 14.25–14.28 v3.47.0–v4.1.3 covering source-safety, profile system, BOOT_WIPE/SYU acks, kernel/sysctl/pkg overhaul, nftables, RADV, _tmpfile_key, argparse --exclusive dispatcher, _write_footer/BLS/exit-triple-form. Phase 6 (73–76, 99–100), Phase 7 (103–106) updated. +45 GROUP D rg patterns. PASS 2 collapsed A–E → 4 subgroups.

v3.9.2  2026-04-06
- 330 checks. Fix 8 Phase-14 rg patterns with escaped `\|` in single quotes (silent false negatives); SYSTEM=12/USER=3/SERVICE=1=16 reconciliation; P14 range 254-324→254-333; BASHISMS rg completion; STEP 4 zip + placeholder guards.

v3.9.1  2026-04-06
- 330 checks. Counter fixes (306→315, 321→330, P14 115→124); PASS 3 table reorder + PASS column; SPECIAL HANDLING expansion; per-phase tally; UNIQUENESS VERIFICATION documented bash-only.

v3.9.0  2026-04-06
- 330 checks (315 static + 15 runtime). Sync ry-install v3.46.0. +9 (325–333): preempt=full, page_lock_unfairness removal, netdev_max_backlog, somaxconn, sysctl priority-99 header, string split -m1, noatime dedup, usbhid.mousepoll removal, coredump.conf.d semantic verify.

v3.8.1  2026-04-05
- 321 checks. Sync ry-install v3.44.0. +20 (305–324): fstab, sysctl, service removals, sudo -n, IPv6, SSID, LVM mask, quoted destinations. +17 GROUP D rg patterns.

v3.8.0  2026-04-05
- 301 checks. +7 Fish 4.0–4.6 compat (298–304). Pin rg ≥ 14.1.1 (bug #2884). Add EVIDENCE RULE, set -uo pipefail, LC_ALL=C.UTF-8, PATH sanitization, umask 077.

v3.7.32  2026-03-18
- 294 checks. +1: sudo -n credential safety in _find_pacnew_files.

v3.7.31  2026-03-18
- 293. Deduplicate _py_extract calls; VERSION guard.

v3.7.30  2026-03-18
- 293. Fix grep -c double-output; remove PCRE2 flag.

v3.7.29–v3.7.23  2026-03-17
- 293 checks. Accumulated fixes: grep fallback, stdout modes, glob handling, budget + timeout tuning, SIZE GATE, dry-run FS monitoring, GROUP E dedup, data mapping, GROUP_EXIT, severity labels, BASH AND-OR rg pattern. v3.7.23 +8 (289–296): fish -c injection, brace expansion, parallel status, glob, wait guard, worker cap, symlink, deprecated test operators.

v3.7.13  2026-03-16
- 285 checks. Unified version scheme (31.x → 3.7.x). 12 rg pattern fixes. Prior lineage: v25 (254 checks) → v31.2.1 (285 checks), 22 releases.
