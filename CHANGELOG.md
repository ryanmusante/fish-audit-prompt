fish-audit-prompt changelog


3.8.1 (2026-04-05)

- 321 checks (306 static + 15 runtime). Sync with ry-install v3.44.0.
- add 20 checks (305–324): fstab, sysctl, irqbalance, service removals, sudo -n, IPv6, SSID, grep separators, TMPDIR guard, string collect, step timing, parallel stderr, LVM mask, validation stderr, quoted destinations.
- update 8 checks: amdgpu-performance, coredump.conf.d, file counts, scope counts, do_logs removal.
- add 17 v3.10–v3.44 pattern searches to GROUP D.
- remove KEY LESSONS section, trim INFRA LESSONS 18→4.
- rename 42 check descriptions for _ry_ prefix.
- fix static checklist count 286→306, total 301→321.

3.8.0 (2026-04-05)

- 301 checks. +7 Fish 4.0–4.6 compatibility checks (298–304).
- add 7 Fish 4.x pattern searches to GROUP D.
- pin rg >= 14.1.1 (false-negative bug #2884).
- add EVIDENCE RULE, set -uo pipefail, LC_ALL=C.UTF-8, python3 -ISc.
- add PATH sanitization, umask 077, trap EXIT/INT/TERM.
- replace SIZE GATE head -500 with skeleton extraction.
- fix checklist count 278→286, Phase 14 87→95.

3.7.32 (2026-03-18)

- 294 checks. +1: sudo -n credential safety in _find_pacnew_files.

3.7.31 (2026-03-18)

- 293 checks. Deduplicate _py_extract calls; add VERSION guard.

3.7.30 (2026-03-18)

- 293 checks. Fix grep -c double-output; remove PCRE2 flag.

3.7.29 (2026-03-17)

- 293 checks. 5 fixes: grep fallback, stdout modes, glob, budget, timeout.

3.7.28 (2026-03-17)

- 293 checks. Sync CHANGELOG with README; add PASS2 checks.

3.7.27 (2026-03-17)

- 293 checks. Expand dry-run FS monitoring; remove GROUP E dupes.

3.7.26 (2026-03-17)

- 293 checks. 6 fixes: SIZE GATE, dry-run FS, rg fallback, timeout, sort, labels.

3.7.25 (2026-03-17)

- 293 checks. 4 fixes: data mapping, GROUP_EXIT, severity, dedup.

3.7.24 (2026-03-17)

- 293 checks. Fix empty alternation in BASH AND-OR rg pattern.

3.7.23 (2026-03-17)

- 293 checks. +8 checks (289–296): fish -c injection, brace expansion, parallel status, glob, wait guard, worker cap, symlink, deprecated test operators.

3.7.13 (2026-03-16)

- 285 checks. Unified version scheme (31.x → 3.7.x). 12 rg pattern fixes.

Prior: v25 (254 checks) → v31.2.1 (285 checks), 22 releases.
