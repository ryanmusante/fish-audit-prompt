fish-audit-prompt changelog

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
