fish-audit-prompt changelog

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
