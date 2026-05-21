fish-audit-prompt ChangeLog
===========================

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
