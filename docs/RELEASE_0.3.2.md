# Theft4 0.3.2 — potential BC texture preparation fix

**Version 0.3.2 · build 144 · iPhone and iPad · 2026-10-14**

This release addresses a confirmed scanner/cache bug behind repeated **“No supported static BC textures were found”** errors. It is a **potential fix** for reports from older phones, including iPhone 12 Pro and iPhone 13 Pro. It does not establish the cause of every report or guarantee older-device compatibility or performance.

**I don't have an older phone, so I cannot test these changes on one. Your mileage may vary (YMMV).**

## What changed

- A scan that finds zero textures is scanned again on retry instead of repeatedly reusing a failed result.
- Restoring or replacing `aes_key.bin` invalidates the saved source scan. Older scan identities are refreshed once; existing converted textures can still be reused by their content keys.
- Empty-scan errors now show the source-file count, resources decoded, warning count and first underlying archive/resource warning.
- **Download Latest Log Capture** now includes a bounded texture scan summary, so reports can distinguish missing sources, unreadable archives and unmatched texture layouts.

The older-device path prepares supported static 2D BC1/BC2/BC3 textures as ASTC. Devices whose GPU supports BC directly skip this requirement. This is not a fix for every graphics or gameplay issue.

## Required game version and exact title update

Use your own legally obtained **Grand Theft Auto IV for Xbox 360, USA retail**, with this exact supported pair:

| Requirement | Supported value |
| --- | --- |
| Title ID | `545407F2` |
| USA retail media ID | `6AC07221` |
| Base executable version | `0.0.0.5` |
| Title update | **GTA IV Title Update 8 (TU8)** matching that base/media ID |
| Update source → target | `0.0.0.5` → `0.0.8.5` |
| Extracted `default.xexp` SHA-256 | `480aee5e2b42707791e7571bb8407c5bb3f6c7534f07f9beb426db4cfc648fd3` |

These are the supported game/update requirements for Theft4, including BC-to-ASTC texture preparation. PC/PS3 assets, another retail revision, an unrelated update, or renamed incompatible files are not substitutes. An ISO working in an emulator does not establish compatibility with Theft4.

Use the [validated staging and installation instructions](IOS_SIDELOAD_INSTALL.md). Copy the prepared folder's contents into the existing **Files → On My iPhone/iPad → Theft4 → game** folder. Keep `default.xex`, the matching `default.xexp`, `aes_key.bin`, and the complete staged archives/resources in their required locations; avoid `game/game`. A raw ISO or unopened update package is not a prepared installation. Executable/update verification alone does not prove every texture source is present or readable.

## Update and retry

1. Download **`Theft4-0.3.2-144-ios-arm64.ipa`** and its checksum from the [release assets](https://github.com/KoreanSeats1/Theft4/releases/tag/v0.3.2). The IPA is unsigned; sign/install it with your compatible sideloading tool.
2. **Update the existing app in place. Do not delete it:** deletion removes its private saves and caches.
3. Open Theft4, check the game files, and retry texture preparation if your GPU requires it. Allow a fresh source scan; completed conversion files remain reusable.
4. Keep Theft4 open during preparation, preferably on power. Allow roughly 2.4 GB for prepared textures plus additional free working space. Pause/resume retains completed conversions.

Requires ARM64 iOS/iPadOS 26.0 or later. This GitHub sideload release is separate from TestFlight.

## Bug reports need logs and information

**Without logs or information I cannot help you with “it doesn't work.”** After a failure, return to the launcher and use **System → Download Latest Log Capture**. Preparation failures do not require starting gameplay or enabling a long performance capture. Attach the exported file to the [Theft4 Tester Log & Bug Submission form](https://docs.google.com/forms/d/e/1FAIpQLScZNq2hjni8wgChCYl1oaRdJU0XPsVLFWwmVtqYxnIQ708t7A/viewform?usp=publish-editor), along with:

- Exact device model, iOS/iPadOS version, and Theft4 version/build.
- Exact error text or a screenshot, the failing setup step, and whether retry changes it.
- Game platform/region/media ID, base version, exact TU8 source/patch hash, and how the game folder was staged and transferred. Do not upload game files or the AES key.
- Free device storage and steps needed to reproduce the problem.

## Validation and limitations

The encrypted-archive regression reproduces a zero-texture scan with a missing key and successful detection after key restoration. Scanner bounds/resume/source-identity tests, ASTC conversion/cache tests, BC capability policy tests, and launcher routing tests pass. The iOS preparation service also passes a syntax check.

**No physical iPhone 12 Pro, iPhone 13 Pro, or other older-phone test was performed for this release.** Host tests and compilation do not confirm the initial cause on an affected phone. Please provide diagnostics if the issue persists.
