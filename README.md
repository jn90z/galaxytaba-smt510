# SM-T510: backup-first LineageOS project

**Status:** Pre-flash. Tablet data has NOT been backed up by this repository and no device has been modified.

## Target
Samsung Galaxy Tab A 10.1 (2019) **SM-T510** (`gta3xlwifi`), not SM-T515.

Provisional ROM: [oddhap LineageOS 21 Android 14 native ARM64 (Nov 12, 2026)](https://github.com/oddhap/android_build_samsung_gta3xlwifi/releases/tag/lineage-21.0-arm64-20261112). The release passes offline checks but physical verification of this exact build was pending. It is **unofficial** and includes older Samsung vendor/kernel components. Do not mix instructions from the user's [XDA GSI thread](https://xdaforums.com/t/rom-unofficial-14-0-sm-t510-arm64-lineageos-21-0-for-galaxy-tab-a-10-1-2019.4804083/) with native ARM64 ZIP install.

## STOP: finish backup first
1. On tablet, write down Settings > About tablet > Model, Software information > Android version, full build number, and CSC/region.
2. Back up photos, Downloads, Documents and other shared files to PC (USB MTP or `adb pull /sdcard/ C:\SM-T510-backup\shared-storage\`). This does **not** copy app-private data.
3. Export app data individually (notes, authenticators, offline games, password vault, messages, downloads). Sync/export contacts and calendars. Smart Switch backup can be supplementary but is **not guaranteed to restore onto LineageOS**.
4. Open a sample of backed-up files and verify the imports/exports. Keep backups offline and **never upload personal data to this public repository**.
5. Retrieve matching SM-T510 stock firmware **after confirming original build, bootloader version and CSC**. Save rollback packages and their hashes to PC.
6. Accept that OEM unlocking wipes device data and **permanently trips Knox**; stock firmware reinstallation will not undo Knox.

### Windows ADB preflight
Install official Android SDK Platform Tools and Samsung USB drivers. Enable USB debugging, authorize PC, then run in PowerShell:

```powershell
adb devices -l
adb shell getprop ro.product.model
adb shell getprop ro.product.device
adb shell getprop ro.build.display.id
adb shell getprop ro.build.version.release
adb shell getprop ro.bootloader
```

Do not publish serial numbers or full logs containing identifiers.

## Native ARM64 release: only after backups verified
**Source of truth:** [INSTALL_ARM64.md](https://github.com/oddhap/android_build_samsung_gta3xlwifi/blob/lineage-21.0-arm64/INSTALL_ARM64.md).

The maintainer expects stock baseline **T510XXU5CWA1 / T510OXM5CVG2**; verify before continuing. Download from the same [release](https://github.com/oddhap/android_build_samsung_gta3xlwifi/releases/tag/lineage-21.0-arm64-20261112):
- `lineage-21.0-20261112-UNOFFICIAL-gta3xlwifi-arm64.zip`
- `twrp-3.7.1_12-gta3xlwifi-arm64-20261015-ap.tar` (Odin AP recovery + vbmeta)
- `SHA256SUMS`; validate with `Get-FileHash -Algorithm SHA256 <downloaded-file>`
- Optionally the unmodified Android 14 **ARM64** MindTheGapps ZIP specified by maintainer.

**Unlock (factory resets):** Enable Developer Options (tap Build number 7×), connect to Wi-Fi, enable OEM unlocking. Power off, hold Volume Up + Volume Down while plugging USB into PC; follow on-screen long-press Volume Up unlock confirmation. Boot stock, reconnect to Internet, verify OEM unlock state.

**TWRP:** Re-enter Download Mode; with Odin put supplied AP tar in AP slot, untick **Auto Reboot** and **Re-Partition**. After PASS, keep USB connected and hold Power + Volume Down until screen goes black, immediately switch to Power + Volume Up; release Power at logo but hold Volume Up until TWRP.

**ROM (wipes again):** TWRP > Wipe > Format Data > yes; Reboot > Recovery. Keep ROM on microSD/OTG or sideload (userdata erased). Keep **Vendor unmounted**; refer to upstream for stopping recovery crypto services if vendor can't be unmounted. Flash the native **ZIP** in TWRP, not a generic GSI system IMG. If desired, flash original Android 14 ARM64 GApps before first boot. Reboot and test Wi-Fi, audio, Bluetooth, cameras, MTP, touch and charging.

**Rollback:** Correct model/CSC and non-downgraded bootloader stock package, Odin BL/AP/CSC, Re-Partition OFF. The native ARM64 ROM writes hybrid vendor; to return to an ARM32 custom ROM, stock CWA1 vendor must first be restored with maintainer's guarded [rollback procedure](https://github.com/oddhap/android_build_samsung_gta3xlwifi/blob/lineage-21.0-arm64/ROLLBACK.md). Do not relock bootloader on custom ROM.

**No flashing is automated here. Do not proceed while any backup/compatibility item remains unverified.**
