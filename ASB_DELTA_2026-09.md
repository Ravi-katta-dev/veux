# September 2026 Android 16 ASB delta — established inputs

## Exact platform changes to integrate

The VoltageOS 16.2 default is AOSP `android-16.0.0_r4`; the security endpoint is
`android-security-16.0.0_r8`.  The table records the r4 and r8 release
revisions plus the relevant commits present in the r8-side release history for
the projects named by VoltageOS's later September maintenance method, filtered
to projects that actually exist in Android 16 r8.  It is a precise Android-16
input list, not an Android-17 import.

| Project | r4 revision | r8 revision | Exact security commit(s) | Decision |
|---|---|---|---|---|
| `platform/development` | `e80a444dddab71488702091314616b470e39bdae` | `3b2c42a42655f575dd29bd7f0c0142f86fe4cd56` | `8b9e6db92ce87408f5f406af903cdaff7aa24df6` — PDU header null checks | Needs AOSP security revision. |
| `platform/external/exfatprogs` | `fb7f6ca1aab4e4be8b109e70c46efffe8c2afd67` | `62e5e8fb8fa08a1dcf90901702e71bfe89e10946` | `c0e2d5cc6945c44781abf764b425cf05cd5c0d8d` — fsck overflow | Needs AOSP security revision. |
| `platform/external/freetype` | `3102764954e3cf4320bb578372a8915e01bd2293` | `23b1472016fc66dd9b435307053b898ec937d776` | `6ca3925a3c974876b5be9396928af3dd37ab1414`, `285b264bf272f4ba115b3ae05a0e4895ca0f82ca` — raster/array overflows | Needs AOSP security revision. |
| `platform/external/libhevc` | `a92aa69d28424d6b968580de05cfca73965b41db` | `8a01d178dfaa82370ce65f5dd075852ad813ec7e` | `5492b022e4d6242e28274cf058c1cf4c5ef1d26e` — decoder heap overflow | Needs AOSP security revision. |
| `platform/external/libpng` | AOSP `0aa733edba01262e2ee7e1622be869ca55cc8e86`; Voltage fork currently `7d6f7a823bb9b22e111da2b3bb3ef443ec828d8a` | `6ec9e36fd0e88e8508f97d6960ce048224420e7b` | `0b9147198ed62548a59b4b427c8b5d512be31249` — NEON palette out-of-bounds | **Exact cherry-pick required** onto the VoltageOS 16.2 fork. It already contains the earlier April libpng fixes, so do not replace the fork with AOSP. |
| `platform/hardware/nxp/secure_element` | `d4a55f96ca839a6758031751789bb2b3af70f383` | `4930b3496de4312262287e48775b2a57eb9903f0` | `466c2e78559e61ca2206bcc07bbb44cab34b25a8` — logical-channel out-of-bounds write | Needs AOSP security revision. |
| `platform/packages/apps/TV` | `7c0cb765acc04fa852f095f6ae07ddcfe5751136` | `5bcd4a384fb6910eebc94f16a538dfb855cc2055` | `856a260c250479dec04ce56cbaace2890123a5e9` — intent redirection | Needs AOSP security revision. |
| `platform/packages/modules/adb` | `7988ec8c2fc39187ec9dd85fad12c3a823427dea` | `2e4a6e15df754fa2905ceb55b6270079a853284e` | `f487f473c82d5bfd90f26e7d7ee9cea0cafde353` — TLS use-after-free | Needs AOSP security revision. |

`libcupsfilters`, `libppd`, `ContactsPicker`, and the other newer-generation
manifest additions are **not applicable**: they have no Android 16 r8 release
tag.  They must not be backfilled from a newer generation.  The observed
VoltageOS September manifest commit remains maintenance-method evidence only.

## Kernel and proprietary disposition

The matching kernel is now established, but its 16.2 HEAD predates the September
release target (2026-06-14).  It contains no demonstrated September security
backport set; kernel CVE/Qualcomm applicability still requires an independent
5.4/SM6375 review before any boot/vendor patch date can change.  The three
required proprietary sources are pinned in the local manifest, but proprietary
blob/firmware fixes also require separate applicability evidence.
