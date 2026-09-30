# Current baseline findings — VoltageOS 16.2 / `veux`

## Workspace fact

`/workspace/veux` is an audit repository, not a checked-out Android source tree:
it contains the four audit artifacts and no `.repo`, `device/xiaomi/veux`,
`kernel/xiaomi/sm6375`, `vendor/xiaomi/veux`, or build output.  The source
revisions below were therefore verified from the specified remotes and their
history, not inferred from a local Android checkout.  No existing source was
discarded or replaced.

| Layer | Exact finding | Status |
|---|---|---|
| VoltageOS | `VoltageOS/manifest:16.2` = `cc66b2c385b93d57a801be2de82e99306c0df134` (2026-07-05). Its `default.xml` points to AOSP `android-16.0.0_r4`. | Frozen comparison baseline. |
| Device | `Karan-Frost/device_xiaomi_veux:sixteen-qpr2` = `4754e23e6ee6cd61236b7030df5c1e37d9b79def` (2026-06-11). | Correct Android 16 QPR2 hardware baseline; do not replace. |
| Device product | `custom_veux` (thus the intended configured lunch is `custom_veux-userdebug`). | Configured by the device tree; no lunch/build has been run in this workspace. |
| Platform ASB endpoint | AOSP manifest `android-security-16.0.0_r8` dereferences to `f4c1bb87279ebd557af20ad303fa889e1c50b68d` (signed tag object `1423022b3492eb9cd639dac73e517abebf4870b5`). | Correct Android 16 security reference. |
| `vendor/xiaomi/veux` | `Karan-Frost/vendor_xiaomi_veux:sixteen-qpr2` = `7b984fe3095d40ddf376403307a7b93c9ee04803` (2026-04-02). | Matching available vendor baseline. |
| Kernel | `VoltageOS-Devices/kernel_xiaomi_sm6375:16.2` = `bd09efb025b4dbfe5b9fea4b441c0a4c844dfbe9` (2026-06-14), Linux `5.4.302-qgki`. It contains `arch/arm64/configs/veux_defconfig` and `veux` DTS files. | **Intended source for `kernel/xiaomi/sm6375`.** The device's `TARGET_KERNEL_CONFIG := veux_defconfig` directly matches it. |
| MIUI camera | `frost-testzone/vendor_xiaomi_miuicamera-veux:main-16` = `9602c4fc6e08fd8600eae1e7eaa8e4b62d2ee766` (2026-04-30). | Required: it provides both `MiuiCamera-veux.mk` and `SEPolicy-veux.mk` included by the device tree. |
| Dolby | `frost-testzone/vendor_oneplus_dolby:main` = `d85136316ef5c84950616d34e5076866c3daffc1` (2026-02-20). | Required: it provides `dolby.mk` and `BoardConfigDolby.mk` included by the device tree. Its HEAD imports Android-16-era OnePlus 11 blobs; no Android-17 variant is selected. |

## Security-property finding

The frozen device configuration sets `BOOT_SECURITY_PATCH := 2025-12-01` and
sets `VENDOR_SECURITY_PATCH` equal to it.  It separately derives the system
vbmeta rollback index from `PLATFORM_SECURITY_PATCH_TIMESTAMP`.  These values
feed different images/rollback behavior and are not proof of source fixes.  Do
not alter either boot/vendor value until the exact kernel and proprietary inputs
are frozen, patched where applicable, built, and verified.
