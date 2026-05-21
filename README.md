# fish-audit-prompt

![version](https://img.shields.io/badge/version-7.4.22-blue)
![checks](https://img.shields.io/badge/checks-454-green)
![phases](https://img.shields.io/badge/phases-15-orange)
![fish](https://img.shields.io/badge/fish-3.6%2B-purple)

Single-source audit specification for `ry-install.fish` v7.4.22. The
prompt drives a zero-skip line-by-line audit across 15 phases (P1-P11
static, P12 runtime, P13 gap, P14 v3.1.0-v7.4.22 specifics, P15
supplemental) totaling 454 checks (443 static + 11 runtime).

## Composition

| File              | Lines | Purpose                                  |
| ----------------- | ----- | ---------------------------------------- |
| fish-audit-prompt.txt | 1503  | Actions-only audit specification         |
| CHANGELOG.md      |   ~50 | Kernel.org-style release log             |
| README.md         |   ~50 | This file                                |

## Phase breakdown

| Phase | Pass | Checks | Topic                                   |
| ----: | ---: | -----: | --------------------------------------- |
|     1 |    1 |     12 | Fish shell compliance                   |
|     2 |    1 |     11 | Stderr/stdout routing                   |
|     3 |    1 |     15 | Error handling & exit codes             |
|     4 |    1 |     12 | Variable handling                       |
|     5 |    1 |     20 | Security                                |
|     6 |    1 |     31 | Embedded config content                 |
|     7 |    1 |     14 | Cross-references & consistency          |
|     8 |    1 |     19 | Function architecture                   |
|     9 |    1 |      9 | Patterns & anti-patterns                |
|    10 |    1 |     11 | Magic numbers & documentation           |
|    11 |    1 |     11 | Testing & validation                    |
|    12 |    2 |     11 | Runtime (PASS 2)                        |
|    13 |    1 |      5 | Gap analysis                            |
|    14 |    1 |    254 | v3.1.0-v7.4.22 specifics                |
|    15 |    1 |     19 | Supplemental deep checks                |
| Total |      |    454 |                                         |

## v7.4.22 baselines (asserted at runtime via `_ir_validate_counts`)

15 invariants: KERNEL_PARAMS:15, MKINITCPIO_HOOKS:11, MKINITCPIO_MODULES:1,
LOGIND_IGNORE_KEYS:9, ENV_VARS:11, SYSCTL_VALUES:16, PKGS_ADD:15, PKGS_DEL:8,
AUR_PKGS:2, MASK:12, EXPECTED_VULKAN_PKGS:3, EXPECTED_SERVICES:3,
_RY_PKG_MANAGED_SERVICES:1, _RY_POST_HOOKS:14, _RY_BOOT_CRITICAL_DSTS:4.

Code metrics: LOC 5083, function count 264, --description coverage 264
(100%), --argument-names 87, set -l 439, bare set 134, set -g in-function
178, set -g top-level 56, `_run\b` 49, `; and ` 318, `; or ` 169,
`string match -qr` 56, `mktemp` 28, command-prefix 241,
_RY_MANAGED_FILE_COUNT 12. Tolerance ±15% per baseline.

## Environment variables (surviving)

`RY_RUN_TIMEOUT`, `RY_INITRD_WARN_MB`, `RY_INSTALL_ALLOW_PARTIAL_UPGRADE`,
`RY_INSTALL_FORCE_BOOT_REBUILD`, `RY_INSTALL_PKG_REMOVE_CASCADE`,
`RY_INSTALL_SKIP_HARDWARE_CHECK`, `RY_INSTALL_WIRELESS_REGDOM`,
`RY_INSTALL_NO_INTERACTIVE_SUDO`, `RY_INSTALL_NO_MATRIX`, `NO_COLOR`.

## Environment variables (retired — regression detectors)

`RY_INSTALL_CONFIRM_BOOT_WIPE`, `RY_INSTALL_CONFIRM_SYSTEM_UPGRADE`.

## Usage

The prompt is consumed by an LLM agent against a copy of `ry-install.fish`
placed at `/home/claude/audit-src/script.fish`. SETUP installs `fish`,
`rg`, `fd`, `bat`; PASS 1 gathers parallel data into
`/home/claude/audit-data/p*.txt`; PASS 2 exercises the script under
runuser drop with NO_COLOR two-axis probes; PASS 3 collates findings into
`fixes-spec-<version>.txt` and ships a store-mode zip to
`/mnt/user-data/outputs/`.

## Phase 14 / Phase 15 ID cross-numbering

Check IDs 217-233 are intentionally shared between Phase 14 (v4.5.x
historical context) and Phase 15 (supplemental deep-check perspective)
following the v7.3.5 prompt lineage. This is not a duplicate-counting
defect; each phase examines the same items from a different audit
dimension.
