  INMO Air3 (IMA301) — ROOT / RE-ROOT GUIDE  (standalone, run from Windows)

  Artifacts are on google drive:
  https://drive.google.com/drive/folders/1eoCtFc_UBIX7V0ZrPNM9ggo1GIoHVqvq?usp=sharing

================================================================================
  
  This kit roots the INMO Air3 by flashing a Magisk-patched boot image.
  Everything below is done by YOU from a Windows PC over USB with fastboot.

  !!! FIRMWARE LOCK — READ FIRST !!!
  These images are built ONLY for firmware:  Air3_DU_V3.17.012_202607091634
  Flashing them on any OTHER firmware version can BRICK the glasses.
  Verify your version first:  Settings > About, OR:  adb shell getprop ro.build.display.id
  It MUST print:  Air3_DU_V3.17.012_202607091634
  If it says anything else, STOP and do not flash.

  Device this was built for:  SN YM00FEF4100109  (SoC SM6450 "parrot", A/B, slot _b)

--------------------------------------------------------------------------------
  WHAT'S IN THIS FOLDER
--------------------------------------------------------------------------------
  magisk_patched_boot.img   <- Magisk 30.7 patched boot (this is what gives root)
  vbmeta_disabled.img       <- vbmeta with AVB verity/verification turned off
  stock_boot.img            <- ORIGINAL unpatched boot (to UN-root / recover)
  stock_vbmeta.img          <- ORIGINAL vbmeta (to fully restore stock verified boot)
  Magisk-v30.7.apk          <- install this APK on the glasses to manage root
  checksums.txt             <- md5 of each image
  ROOT_GUIDE.txt            <- this file

--------------------------------------------------------------------------------
  ONE-TIME SETUP ON THE PC
--------------------------------------------------------------------------------
  1. Install Google's platform-tools (adb + fastboot) if you don't have it:
       https://developer.android.com/tools/releases/platform-tools
     Unzip it, e.g. to  C:\platform-tools\
  2. Install the Google USB / Android bootloader driver if Windows doesn't see
     the device in fastboot (Device Manager should show "Android Bootloader
     Interface" once in fastboot mode).
  3. Copy the 4 .img files from THIS folder into your platform-tools folder
     (so the commands below find them), OR just cd into this folder in a
     terminal that has fastboot on PATH.

  Open a Command Prompt / PowerShell in the folder that has fastboot AND the
  .img files. Test it:
       fastboot --version
       adb devices        (with glasses booted + USB debugging on -> shows a device)

================================================================================
  PATH A — RE-FLASH ROOT ON AN ALREADY-UNLOCKED DEVICE   (NO DATA WIPE)
================================================================================
  Use this if the bootloader is already unlocked (this unit already is:
  orange boot state). This does NOT erase your data. Use it to re-apply root
  after an update reverted the boot, or if root broke.

  1. Boot the glasses normally, enable USB debugging, plug into the PC.
  2. Reboot into the bootloader (fastboot):
       adb reboot bootloader
     (or power off, then hold the volume key(s) + plug USB per INMO's combo)
  3. Confirm the PC sees it:
       fastboot devices          (should list a serial + "fastboot")
  4. Flash the patched boot to the ACTIVE slot:
       fastboot flash boot magisk_patched_boot.img
  5. Flash the AVB-disabled vbmeta (flash it PLAIN — do NOT add
     --disable-verity/--disable-verification, that fails on this device):
       fastboot flash vbmeta vbmeta_disabled.img
  6. Reboot:
       fastboot reboot
  7. First boot after flashing can take a couple of minutes. When booted,
     install Magisk-v30.7.apk on the glasses to manage root.

================================================================================
  PATH B — FIRST-TIME ROOT ON A LOCKED / FRESH DEVICE   (ERASES ALL DATA!)
================================================================================
  Use this only if the bootloader is LOCKED (green boot state). Unlocking
  FACTORY-RESETS the glasses (wipes everything). Back up anything you need first.

  1. On the glasses: Settings > About > tap Build number 7x to enable Developer
     options. Then Settings > System > Developer options > enable BOTH:
        - USB debugging
        - OEM unlocking          (must be ON or unlock will be refused)
  2. Reboot to bootloader:
       adb reboot bootloader
  3. (Optional sanity check — should print 1)
       fastboot flashing get_unlock_ability
  4. Unlock (THIS WIPES ALL USER DATA):
       fastboot flashing unlock
     Follow the on-screen prompt on the glasses to confirm (volume to select,
     power to accept). The device wipes and reboots. Let it finish setup, then
     redo steps 1-2 above (debugging + reboot bootloader) because the wipe
     reset those toggles.
  5. Flash root + vbmeta:
       fastboot flash boot magisk_patched_boot.img
       fastboot flash vbmeta vbmeta_disabled.img
  6. Reboot:
       fastboot reboot
  7. Install Magisk-v30.7.apk on the glasses.

  NOTE: after unlocking, boot state is permanently "orange" and hardware
  attestation (strong Play Integrity) fails — that's normal for any unlocked
  Android. Basic integrity still works with Magisk + Play Integrity Fix, so the
  Play Store and most apps keep working.

================================================================================
  UN-ROOT / RECOVERY
================================================================================
  Remove root but stay unlocked (revert to stock boot):
       fastboot flash boot stock_boot.img
       fastboot reboot

  Fully restore stock verified boot (re-enable AVB):
       fastboot flash boot stock_boot.img
       fastboot flash vbmeta stock_vbmeta.img
       fastboot reboot
     (You may then re-lock with `fastboot flashing lock` — this WIPES again and
      only works cleanly if boot+vbmeta are fully stock, else it can bootloop.
      Re-locking is optional and NOT required.)

  If a flash goes wrong and it won't boot:
   - You're on an A/B device; fastboot itself is the safety net. Just re-flash
     stock_boot.img (and stock_vbmeta.img) from fastboot and reboot.
   - Worst case, re-flash the full V3.17.012 firmware.

================================================================================
  TROUBLESHOOTING
================================================================================
  - `fastboot devices` shows nothing: install the Android Bootloader driver
    (Device Manager > right-click the unknown/other device > Update driver >
    browse to the Google USB driver), try a different USB port / a data cable.
  - "FAILED (remote: ... not allowed)" on unlock: OEM unlocking toggle is off,
    or get_unlock_ability returned 0 — re-enable it in Developer options.
  - Boot loops after flashing boot: you flashed the wrong firmware's boot, or
    skipped vbmeta_disabled.img. Flash stock_boot.img + stock_vbmeta.img to
    recover, and double-check ro.build.display.id == V3.17.012.
  - Verify what you flashed matches (md5 in checksums.txt).
================================================================================


A little support goes a long way! If you’d like to help me keep creating, you can do so at https://ko-fi.com/j4ckgrey
<img width="1200" height="600" alt="image" src="https://github.com/user-attachments/assets/3bfba3a1-916a-4e5a-b958-22eef30bd169" />
