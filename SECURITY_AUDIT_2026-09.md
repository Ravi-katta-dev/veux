# Security audit — September 2026

## Current baseline

| Item | Finding |
|---|---|
| Device | `Karan-Frost/device_xiaomi_veux:sixteen-qpr2` at `4754e23e6ee6cd61236b7030df5c1e37d9b79def`; preserved without modification. |
| Kernel | No exact Android 16/QPR2 remote or SHA established. Required source path: `kernel/xiaomi/sm6375`. |
| Vendor | `vendor/xiaomi/veux` at `7b984fe3095d40ddf376403307a7b93c9ee04803`; MIUI camera and Dolby revisions unresolved. |
| VoltageOS | Manifest `16.2` at `cc66b2c385b93d57a801be2de82e99306c0df134`, default AOSP revision `android-16.0.0_r4`. |
| Build target | `custom_veux-userdebug` is defined by the device tree. No source workspace or build result exists here. |
| Baseline SPL | Platform/build artifact unavailable. Device boot/vendor source values are `2025-12-01`. |

## September ASB

AOSP `android-security-16.0.0_r8` is the correct Android 16 reference.  No
security source has been modified, no Android 17 source has been introduced,
and no SPL metadata has been bumped.  Therefore the final SPL is **not
verified** and must not be advertised as `2026-09-05`.

## Required release evidence

| Bucket | Required change/evidence | Current state |
|---|---|---|
| Platform/framework | Traceable r4→r8 ASB commits integrated with VoltageOS-specific changes preserved; resolved `repo manifest -r`. | Not performed: checkout absent. |
| Kernel | Exact matching SHA, applicability decisions, patches, build and boot evidence. | Blocked on provenance; no guessed kernel selected. |
| Vendor | Exact commits for `veux`, MIUI camera, and Dolby; proprietary/Qualcomm applicability decision. | Only `veux` is frozen. |
| Device tree | Compatibility-only changes, if a proven build conflict requires them. | None required or made. |
| Build | Baseline successful `mka bacon`, then patched successful `mka bacon`, with artifact hashes. | Not run: no Android tree. |
| Properties | `ro.build.version.security_patch` and `ro.vendor.build.security_patch` from the produced image. | Not available. |

## Guardrails

The manifest overlay contains no newer-generation source declarations.  Keep the
QPR2 device branch fixed, do not advance boot/vendor security properties merely
to match a framework date, and reject any patch that cannot be tied to the
Android 16 reference or an applicable lower-layer fix.
