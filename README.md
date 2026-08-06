# INMO Air3 (IMA301) Root Guide

Root and re-root guide for the **INMO Air3 (IMA301)** using a **Magisk-patched boot image**.

> **Platform:** Windows + Fastboot (ADB)

---

# ⚠️ Important: Firmware Compatibility

**These files are ONLY compatible with the following firmware:**

```
Air3_DU_V3.17.012_202607091634
```

Flashing these images on **any other firmware version may brick your device.**

Verify your firmware before doing anything:

### On the device

```
Settings → About
```

### Or via ADB

```bash
adb shell getprop ro.build.display.id
```

It **must** return:

```text
Air3_DU_V3.17.012_202607091634
```

If it doesn't, **stop here**.

---

## Device Information

* **Model:** INMO Air3 (IMA301)
* **SoC:** Qualcomm SM6450 ("parrot")
* **Partition Layout:** A/B
* **Target Slot:** `_b`

---

# Downloads

All required files are available here:

**Google Drive**

https://drive.google.com/drive/folders/1eoCtFc_UBIX7V0ZrPNM9ggo1GIoHVqvq?usp=sharing

---

# Included Files

| File                      | Description                           |
| ------------------------- | ------------------------------------- |
| `magisk_patched_boot.img` | Magisk 30.7 patched boot image (root) |
| `vbmeta_disabled.img`     | vbmeta with AVB verification disabled |
| `stock_boot.img`          | Original boot image                   |
| `stock_vbmeta.img`        | Original vbmeta                       |
| `Magisk-v30.7.apk`        | Magisk Manager APK                    |
| `checksums.txt`           | MD5 checksums                         |
| `ROOT_GUIDE.md`           | This guide                            |

---

# PC Requirements

Install the latest Android Platform Tools:

https://developer.android.com/tools/releases/platform-tools

Example installation path:

```
C:\platform-tools\
```

Copy the four `.img` files into the Platform Tools folder.

Verify everything is working:

```bash
fastboot --version
adb devices
```

Your device should appear while Android is running with **USB Debugging enabled**.

---

# Path A — Re-Root an Already Unlocked Device

> **No data will be erased**

Use this if your bootloader is already unlocked (orange boot state).

### 1. Boot normally

Enable:

* USB Debugging

Connect the device to your PC.

---

### 2. Enter Fastboot

```bash
adb reboot bootloader
```

---

### 3. Verify connection

```bash
fastboot devices
```

---

### 4. Flash Magisk boot

```bash
fastboot flash boot magisk_patched_boot.img
```

---

### 5. Flash vbmeta

**Do NOT use**

```
--disable-verity
--disable-verification
```

Simply flash:

```bash
fastboot flash vbmeta vbmeta_disabled.img
```

---

### 6. Reboot

```bash
fastboot reboot
```

First boot may take a few minutes.

---

### 7. Install Magisk

Install:

```
Magisk-v30.7.apk
```

Root should now be active.

---

# Path B — First-Time Root (Locked Bootloader)

> **Warning**
>
> Unlocking the bootloader **completely wipes the device**.

Back up anything important before continuing.

---

## 1. Enable Developer Options

Settings →

About →

Tap **Build Number** seven times.

Enable:

* USB Debugging
* OEM Unlocking

---

## 2. Enter Fastboot

```bash
adb reboot bootloader
```

---

## 3. (Optional) Verify unlock capability

```bash
fastboot flashing get_unlock_ability
```

Expected result:

```
1
```

---

## 4. Unlock Bootloader

```bash
fastboot flashing unlock
```

Confirm the unlock on the device.

The device will:

* erase all user data
* reboot

After setup, re-enable:

* USB Debugging
* OEM Unlocking

Then reboot back into Fastboot.

---

## 5. Flash Root

```bash
fastboot flash boot magisk_patched_boot.img
fastboot flash vbmeta vbmeta_disabled.img
```

---

## 6. Reboot

```bash
fastboot reboot
```

---

## 7. Install Magisk

Install:

```
Magisk-v30.7.apk
```

Done!

---

# Unroot

Remove Magisk while keeping the bootloader unlocked:

```bash
fastboot flash boot stock_boot.img
fastboot reboot
```

---

# Restore Completely Stock

Restore verified boot:

```bash
fastboot flash boot stock_boot.img
fastboot flash vbmeta stock_vbmeta.img
fastboot reboot
```

If desired, you can then relock:

```bash
fastboot flashing lock
```

> **Warning**
>
> Relocking wipes the device again.
>
> Only relock if both `boot` and `vbmeta` are completely stock.

---

# Recovery

If the device won't boot:

1. Boot into Fastboot.
2. Flash:

```bash
fastboot flash boot stock_boot.img
fastboot flash vbmeta stock_vbmeta.img
fastboot reboot
```

Worst case, reinstall the official firmware:

```
Air3_DU_V3.17.012_202607091634
```

---

# Troubleshooting

### `fastboot devices` shows nothing

* Install the Google Android Bootloader Driver
* Try another USB cable
* Try another USB port
* Verify Device Manager detects **Android Bootloader Interface**

---

### Unlock not allowed

```
FAILED (remote: ... not allowed)
```

Make sure:

* OEM Unlocking is enabled
* `fastboot flashing get_unlock_ability` returns `1`

---

### Bootloop after flashing

Usually caused by:

* flashing images from the wrong firmware
* skipping `vbmeta_disabled.img`

Recover using:

```bash
fastboot flash boot stock_boot.img
fastboot flash vbmeta stock_vbmeta.img
```

---

### Verify downloads

Compare the files against:

```
checksums.txt
```

---
---

# 💬 Community & Support

<table>
<tr>
<td width="55%" valign="top">

## Community

Need help or want to chat with other INMO users?

**Discord**

https://discord.com/invite/4yB8URK9s

Issues, discoveries, and new firmware findings are always welcome.

<br>

## ❤️ Support the Project

If this guide saved you time or helped you get your Air3 rooted, consider supporting future development.

☕ **Buy me a coffee**

https://ko-fi.com/j4ckgrey

<td width="45%" align="center">

<a href="https://ko-fi.com/j4ckgrey">
  <img
    src="https://github.com/user-attachments/assets/7375555f-b605-4f14-872a-f4613114bdea"
    alt="Support on Ko-fi"
    width="500">
</a>

</td>
</tr>
</table>
