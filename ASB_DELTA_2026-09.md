# September 2026 ASB findings — Android 16 / QPR2

## Actual platform comparison

* **VoltageOS base:** `android-16.0.0_r4` via manifest commit
  `cc66b2c385b93d57a801be2de82e99306c0df134`.
* **September reference:** `android-security-16.0.0_r8`, resolved manifest
  commit `f4c1bb87279ebd557af20ad303fa889e1c50b68d`.
* **Platform/build example:** AOSP `platform/build` is
  `eb52880e37bf8fc962254cac514a3ae1545595e0` at r4 and
  `4af8baddee035f1183428ebf9687e028ef2dfec8` at r8 (418 commits in that
  range).  This demonstrates why a date-only property edit is invalid.

There is no resolved VoltageOS Android 16 source checkout in this workspace,
so an exact per-project applied-commit list cannot be claimed.  The next source
operation is a real `repo manifest -r` of the 16.2 workspace, followed by a
per-project r4..r8 comparison; it is **not** a full manifest rebase.

## VoltageOS maintenance method actually observed

VoltageOS's 16.2 branch used project-level forks for its June ASB maintenance:
manifest commit `daca559469f5ecd16ef36c905920905541b010c6` removed AOSP
projects and re-added its own `16.2` forks for `external/libpng`,
`external/sqlite`, `packages/apps/CertInstaller`, and `packages/apps/KeyChain`.
Those four fork declarations remain in the current 16.2 manifest.  This is the
method to reuse: retain a VoltageOS fork only when it contains the required
Android 16 security commits, otherwise use the minimal AOSP revision or a
reviewed cherry-pick.

The September maintenance commit on the newer VoltageOS generation is
`6f5b3dcf056b04eca49bd34e1db7512190faec1b`.  It forks development, exfatprogs,
freetype, libcupsfilters, libhevc, libpng, libppd, NXP secure-element,
ContactsPicker, TV, and adb.  It is evidence of the *manifest fork method only*:
it must not be copied into 16.2, because most corresponding VoltageOS repositories
have no public `16.2` branch and it is not an Android 16 commit ledger.

## Required decision ledger before changes

| Bucket | Current decision | Required next evidence |
|---|---|---|
| Android platform/framework | Pending r4→r8 project comparison. Existing 16.2 forks above must be checked first. | Upstream SHA, CVE/ASB association, selected source (already sufficient / AOSP / VoltageOS fork / LineageOS reference / cherry-pick), resulting SHA. |
| Kernel | Pending. | Exact `android_kernel_xiaomi_sm6375` remote+SHA and applicable Android/Linux/Qualcomm patch set. |
| Vendor/Qualcomm/proprietary | `vendor/xiaomi/veux` frozen; camera and Dolby sources unknown. | Exact source+SHA for all three required vendor paths and applicability assessment for firmware/SoC components. |

A LineageOS fork may be used only as an upstream-reference comparison after the
Android 16 kernel/vendor provenance is known; it is not a replacement source.
