fish-audit-prompt ChangeLog
===========================

v7.4.23 - 2026-05-20
--------------------

Prompt-internal audit pass. The script under test (ry-install.fish) is
unchanged at v7.4.22; only the audit specification is bumped.

Baselines resynced against the actual v7.4.22 source. Three counts in
the header line and the README metrics table drifted outside the ±15%
tolerance and are now corrected: `set -l` 439 → 450 (+2.5%), `set -g`
in-function 178 → 151 (-15.2%), and command-prefix 241 → 200 (-17.0%).
The remaining 11 baselines verify exact. The `_ir_validate_counts`
15-entry invariant set is unchanged.

SETUP STEP 1 ripgrep version pin upgrade path is now gated behind
`command -v dpkg` plus `dpkg --print-architecture = amd64`. The previous
unconditional `dpkg -i ripgrep_15.0.1-1_amd64.deb` would silently fail
on Arch, Fedora, and ARM hosts where dpkg is absent or amd64 is not
the runtime architecture; the gate now emits a `[WARN] rg <ver> < 14.1.1
— pin available only for dpkg+amd64; install rg >= 14.1.1 manually`
diagnostic instead, leaving the older rg in place rather than aborting
setup. Functional fallback paths (apt-get / pacman / dnf installs) are
unchanged.

EXECUTION block restructured: RULES, EVIDENCE RULE, and TOOL MANDATES
collapsed into a single RULES + TOOLS pair, eliminating the cross-block
duplication of "Batch + parallel" and "rg > grep" between TOOL MANDATES
and the earlier RULES section. The "CONTEXT BUDGET (3000 lines): PASS 1
800 · PASS 2 600 · PASS 3 200 · Reasoning 1400" line is retired; the
fixed-budget assumption was incorrect for current-generation model
contexts, and the operational rule "raw data → disk; summary → context"
is preserved verbatim under the renamed CONTEXT key.

Header policy line "Policy: ZERO-SKIP. Every source line. Trace error
branches, unreachable code, early returns. No sampling." rephrased to
"Policy: ZERO-SKIP — every line, every branch, no sampling." The
operational meaning is identical; the wording is one line shorter and
matches the actions-only convention adopted in v7.4.22.

Minor consistency fixes: STEP 3 checklist-count verify line shortened
from "→ 443 (P12's 11 runtime checks inline in PASS 2; total 454)" to
"= 443 (+ 11 runtime in PASS 2 = 454)"; STEP 2 ANALYZE subheadings
"Missing data" / "Report to context" promoted to uppercase MISSING DATA
/ REPORT TO CONTEXT to match the file's existing capitalization
convention; data-collection preamble "All rg capped with | head -N"
reworded to "Cap each multi-line rg with | head -N" to match the
imperative voice used elsewhere.

Static check total 443 + runtime 11 = 454, unchanged. Phase 14 (254
checks) and Phase 15 (19 checks) ID layouts unchanged. The
intentional 217-233 ID cross-numbering between P14 historical context
and P15 deep-check perspective remains documented in README.md.


v7.3.5 - v7.4.22 - 2026-05-20
-----------------------------

Audit prompt resynced against ry-install v7.4.22 baseline (LOC 5083,
function count 264, --description coverage 100%, all 15 `_ir_validate_counts`
invariants unchanged from v7.3.5). The prompt body was stripped of historical
resync paragraphs, per-check version rationale, and lineage narratives;
remaining content is actions-only. Every static and runtime check now reads
as a single imperative line keyed to source-truth verifiable by rg/python3/fish
invocations.

Phase 14 expanded to cover v7.4.x deltas: 23 new check IDs (331-425, plus
fillers) for the v7.4 run-summary matrix subsystem (_phase_record,
_rdi_render_matrix, _rdi_matrix_header/rows/footer, _rdi_elapsed,
_rdi_summary, _RY_PHASE_RESULTS, _RY_MTX_* tally globals), the
_RY_BOOT_CRIT_HIT dedicated sentinel (replacing the overloaded
_PROG_FINALIZED_SKIP flag), the RY_INSTALL_NO_INTERACTIVE_SUDO and
RY_INSTALL_NO_MATRIX environment opt-outs, _ry_sudo_cache_banner
(replacing the strict NOPASSWD ALL preflight gate), _acquire_lock
PID-recycle race close via /proc/$pid/comm, _acquire_lock_fresh umask
0077 around mkdir, _cleanup_tmpfiles two-step `sudo -n true` gate,
_run_emit_stream head-100 + tail-100 capture (build-error tails preserved),
_boot_initrd_size_scan byte comparison (no off-by-1MB silent pass),
_ry_check_kernel_version rc=0/1/2 three-way dispatch (only rc=2 elevates
INSTALL_HAD_ERRORS), GOVERNOR-uppercase rename in cpupower-service.conf,
TMPDIR non-existent fallback to /tmp with stderr warning, 9-way
is-enabled chain collapsed to a single `contains` check, 76 adjacent
`set` runs collapsed to semicolon-chained one-liners, the v7.4.20 helper
extractions (_ip_record_regdom, _iap_per_pkg_retry, _iap_record_result,
_irb_taint_gate, _rrp_optional_indexer, _ip_bail_prep, _irb_skip_post_mki),
and mt7925 modinfo failure routing to WARN rather than silent PASS.

Phase 6 check count corrected from 30 to 31; the v7.3.5 header
miscounted the actual 71-101 range. Phase 14 check count adjusted from
255 to 254 after the two stale P14.18 cross-reference placeholders
(215, 216 — which forwarded to P15) were deleted; P15 retains those
IDs as the canonical definition. Phase 14/15 ID cross-numbering for
217-233 preserved from v7.3.5 lineage: P14 defines the historical
v4.5.x context, P15 supplies the deep-check perspective on the same
items.

Static check total 443 + runtime 11 = 454, matching the summary
table. The prompt remains the single-source-of-truth specification
for full v7.4.22 audits.


v7.3.5 - 2026-05-17
-------------------

Last v7.3.x audit prompt. 431 checks (420 static + 11 runtime).
Derived from ry-install.fish v7.3.5. Phase 14 covered v3.1.0-v7.3.5
specifics with 232 checks; Phase 15 added 19 supplemental deep-check
items reusing IDs 215-233 from the P14 namespace by design. CHANGELOG
emitted in legacy bullet format; superseded by the kernel.org prose
style adopted in v7.4.22.
