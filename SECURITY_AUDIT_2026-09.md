# Security audit — September 2026

## Current baseline

| Item | Finding |
|---|---|
| Device | `Karan-Frost/device_xiaomi_veux:sixteen-qpr2` at `4754e23e6ee6cd61236b7030df5c1e37d9b79def`; preserved without modification. |
| Kernel | `VoltageOS-Devices/kernel_xiaomi_sm6375:16.2` at `bd09efb025b4dbfe5b9fea4b441c0a4c844dfbe9`; Linux `5.4.302-qgki`, `veux_defconfig`, and veux DTS support prove it matches `kernel/xiaomi/sm6375`. |
| Vendor | `vendor/xiaomi/veux` `7b984fe3095d40ddf376403307a7b93c9ee04803`; MIUI camera `9602c4fc6e08fd8600eae1e7eaa8e4b62d2ee766`; Dolby `d85136316ef5c84950616d34e5076866c3daffc1`. |
| VoltageOS | Manifest `16.2` at `cc66b2c385b93d57a801be2de82e99306c0df134`, default AOSP revision `android-16.0.0_r4`. |
| Build target | `custom_veux-userdebug` is defined by the device tree. No source workspace or build result exists here. |
| Baseline SPL | Platform/build artifact unavailable. Device boot/vendor source values remain `2025-12-01`. |

## September ASB

`ASB_DELTA_2026-09.md` now lists the exact Android 16 r4→r8 revisions and the
security commits for the identified affected projects.  The manifest pins the
matching hardware repositories and advances only the seven un-forked AOSP
projects to their Android 16 r8 tags.  `external/libpng` remains on the
VoltageOS 16.2 fork pending the one documented cherry-pick.

## Remaining unknowns — release blockers only

1. **Kernel/Qualcomm security delta:** the matching 5.4 kernel is identified,
   but no September applicability/commit ledger has yet been established.
2. **Proprietary firmware/SoC delta:** the blob repositories are identified,
   but no evidence yet maps their firmware to September fixes.
3. **Integrated build evidence:** apply the manifest changes and libpng
   cherry-pick in a real VoltageOS workspace, then run `mka bacon` and capture
   `ro.build.version.security_patch` and `ro.vendor.build.security_patch`.

Until all three exist, the final SPL is **not verified** and must not be
advertised as `2026-09-05`; no SPL variable has been changed.
