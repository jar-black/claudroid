# Installation Guide: Custom Android on Xiaomi Redmi Note 12 Pro 5G

## ⚠️ IMPORTANT WARNINGS

**READ THIS CAREFULLY BEFORE PROCEEDING:**

1. **Data Loss**: All installation procedures will WIPE ALL DATA from your phone. Backup everything important first.
2. **Warranty**: Installing custom firmware VOIDS your warranty.
3. **Risk of Bricking**: Incorrect procedures can permanently damage your device.
4. **No Guarantee**: These instructions are provided as-is. Proceed at your own risk.
5. **Backup Your IMEI/EFS**: Before proceeding, backup your IMEI and EFS partition. Losing this data can permanently disable cellular connectivity.

## Prerequisites Checklist

Before starting, ensure you have:

- [ ] Xiaomi Redmi Note 12 Pro 5G (codename: ruby, model 22101316G for EU/Sweden)
- [ ] **Bootloader is unlocked** (using Xiaomi's official unlock tool)
- [ ] Windows PC (or Linux with appropriate tools)
- [ ] USB cable (original Xiaomi cable recommended)
- [ ] At least 70% battery charge on your phone
- [ ] Complete backup of all important data
- [ ] Latest USB drivers installed
- [ ] Android SDK Platform Tools installed
- [ ] 2-3 hours of uninterrupted time

## Step 1: Verify Bootloader is Unlocked

### Check Bootloader Status

1. Power off your phone
2. Boot into Fastboot mode:
   - Hold **Volume Down + Power** buttons together
   - Release when you see the Mi Bunny logo with "FASTBOOT" text
3. Connect phone to PC via USB
4. Open Command Prompt/Terminal in Platform Tools folder
5. Run:
   ```bash
   fastboot devices
   ```
   - You should see your device serial number
6. Check unlock status:
   ```bash
   fastboot oem device-info
   ```
   - Look for: `Device unlocked: true`

### If Bootloader is NOT Unlocked

You must unlock it first using Xiaomi's official tool:

1. Go to: https://en.miui.com/unlock/
2. Download Mi Unlock Tool
3. Apply for unlock permission (may take 7-30 days wait period)
4. Follow Xiaomi's official unlock process
5. **WARNING**: Unlocking will erase ALL data

## Step 2: Install Required PC Software

### Windows Users

1. **Android SDK Platform Tools**:
   - Download from: https://developer.android.com/studio/releases/platform-tools
   - Extract to: `C:\platform-tools\`
   - Add to PATH or navigate to this folder when running commands

2. **USB Drivers**:
   - Install Xiaomi USB drivers from: https://xiaomiflashtool.com/
   - Or use universal ADB drivers

3. **Verify Installation**:
   ```cmd
   fastboot --version
   adb version
   ```

### Linux Users

```bash
# Ubuntu/Debian
sudo apt-get update
sudo apt-get install android-tools-adb android-tools-fastboot

# Arch Linux
sudo pacman -S android-tools

# Verify
fastboot --version
adb version
```

## Step 3: Backup Everything

### Create Complete Backup

1. **Photos, Videos, Documents**:
   - Copy to your PC or cloud storage

2. **Contacts and Messages**:
   - Export contacts to Google account
   - Use SMS Backup apps

3. **App Data**:
   - Use built-in MIUI backup feature
   - Or use third-party backup apps

4. **IMPORTANT - Backup IMEI/EFS** (Advanced):
   ```bash
   adb shell
   su
   dd if=/dev/block/bootdevice/by-name/modemst1 of=/sdcard/modemst1.bin
   dd if=/dev/block/bootdevice/by-name/modemst2 of=/sdcard/modemst2.bin
   dd if=/dev/block/bootdevice/by-name/fsg of=/sdcard/fsg.bin
   exit
   exit
   adb pull /sdcard/modemst1.bin
   adb pull /sdcard/modemst2.bin
   adb pull /sdcard/fsg.bin
   ```
   **Store these files safely - they contain your IMEI!**

## Step 4: Download Required Files

### For Custom Recovery Installation

1. **TWRP or OrangeFox Recovery** (Choose one):

   **TWRP for Ruby** (Unofficial):
   - Source: XDA Forums - "Recovery TWRP and OrangeFox for Redmi Note 12 Pro 5G (RUBY)"
   - URL: https://xdaforums.com/t/recovery-twrp-and-orangefox-for-redmi-note-12-pro-5g-ruby.4668092/
   - File format: `.img` file (usually named like `twrp-3.x.x-ruby.img`)

   **OrangeFox Recovery** (Alternative):
   - Check same XDA thread above
   - More modern UI, similar functionality to TWRP

   **Alternative Source**:
   - Xiaomi Tools: https://xiaomitools.com/download/redmi-note-12-pro-plus-5g-ruby-twrp/

### For Custom ROM Installation

**Option 1: LineageOS 21 (Android 14)** - Recommended

1. **ROM File**:
   - Source: bengris32's releases on GitHub
   - URL: https://github.com/bengris32/releases/releases
   - Look for: `lineage-21.0-*-ruby.zip`
   - Size: ~1-1.5 GB

2. **Lineage Recovery Boot Image**:
   - Download `boot.img` from same GitHub release
   - **REQUIRED**: Must use Lineage Recovery to flash the ROM

3. **Firmware Requirements**:
   - MIUI 14.0.6.0 TMOMIXM firmware (for LineageOS 21)
   - Download from: https://xiaomirom.com/en/rom/redmi-note-12-pro-pro-pro-5g-discovery-5g-ruby-taiwan-fastboot-recovery-rom/

4. **Optional Add-ons**:
   - Google Apps (GApps): https://github.com/MindTheGapps/MindTheGapps-Android14-arm64/releases
   - Magisk (for root): https://github.com/topjohnwu/Magisk/releases

**Option 2: Other Custom ROMs**

- **crDroid**: Check XDA Forums for ruby builds
- **Pixel Experience**: Available on XDA Forums
- **ArrowOS**: Check XDA Forums

### Verify File Integrity

After downloading:
1. Check file sizes match what's listed
2. Verify MD5/SHA checksums if provided
3. DO NOT use corrupted or incomplete downloads

## Step 5: Install Custom Recovery

### Method A: Temporary Boot (Safer - Recommended)

This method boots recovery without permanently installing it, allowing you to test first:

1. **Boot to Fastboot Mode**:
   - Power off phone
   - Hold **Volume Down + Power**
   - Connect to PC via USB

2. **Temporarily Boot TWRP**:
   ```bash
   fastboot boot twrp-ruby.img
   ```
   (Replace `twrp-ruby.img` with your actual recovery filename)

3. **Phone will boot into TWRP**:
   - This is temporary - when you reboot, stock recovery returns
   - But you can now flash the ROM

### Method B: Permanent Installation

This permanently replaces stock recovery:

1. **Boot to Fastboot Mode**:
   - Power off phone
   - Hold **Volume Down + Power**
   - Connect to PC via USB

2. **Flash Recovery to Recovery Partition**:
   ```bash
   fastboot flash recovery twrp-ruby.img
   ```

3. **Reboot to Recovery**:
   ```bash
   fastboot reboot recovery
   ```

   **IMPORTANT**: Do NOT boot to system yet! Go directly to recovery!

4. **Alternative Manual Reboot**:
   - Unplug USB
   - Hold **Volume Up + Power** to boot to recovery

### Method C: Using LineageOS Boot Image (For LineageOS)

If installing LineageOS, use their recovery:

1. **Boot to Fastboot Mode**

2. **Flash Lineage Recovery Boot Image**:
   ```bash
   fastboot flash boot boot.img
   ```
   (Use the `boot.img` from LineageOS release)

3. **Reboot to Recovery**:
   ```bash
   fastboot reboot recovery
   ```

## Step 6: Flash Custom ROM

### Preparation in Recovery

1. **In TWRP/OrangeFox**:
   - Wipe > Advanced Wipe
   - Select: **Dalvik/ART Cache, Cache, Data, System**
   - DO NOT wipe: Internal Storage (unless you want to lose files)
   - Swipe to confirm wipe

2. **For LineageOS Recovery**:
   - Select "Factory Reset"
   - Select "Format data/factory reset"
   - Confirm

### Transfer ROM File to Phone

**Method 1: ADB Sideload** (Recommended):

1. In Recovery, select "Advanced" > "ADB Sideload" (TWRP)
   - Or "Apply Update" > "Apply from ADB" (Lineage Recovery)

2. On PC:
   ```bash
   adb sideload lineage-21.0-*-ruby.zip
   ```

3. Wait for transfer and installation (10-20 minutes)

**Method 2: USB Storage**:

1. In TWRP: Mount > Enable MTP
2. Copy ROM zip to phone's internal storage
3. In TWRP: Install > Select ROM zip > Swipe to flash

### Flash Additional Files (Optional)

After flashing ROM, also flash (in order):

1. **GApps** (if wanted):
   ```bash
   adb sideload MindTheGapps-*.zip
   ```

2. **Magisk** (for root):
   ```bash
   adb sideload Magisk-*.apk
   ```
   (Rename .apk to .zip before flashing)

### Important Notes for LineageOS

- **MUST use Lineage Recovery** (boot.img) to flash LineageOS ROM
- TWRP may cause issues with LineageOS installation
- Ensure you have correct firmware base (MIUI 14.0.6.0 TMOMIXM)

## Step 7: First Boot

### Booting Up

1. In Recovery, select "Reboot System"
   - Or: "Reboot" > "System"

2. **First boot takes 5-15 minutes**:
   - Phone will show boot logo for extended time
   - Be patient! DO NOT interrupt!
   - If boot takes more than 20 minutes, something may be wrong

3. **If boot loops**:
   - Boot back to recovery
   - Wipe cache and dalvik
   - Try rebooting again
   - If still fails, re-flash ROM

### Initial Setup

1. **Select Language and Region**: Sweden
2. **Wi-Fi**: Connect to network
3. **Google Account**: Sign in (if using GApps)
4. **Complete setup wizard**

5. **Recommended First Steps**:
   - Check Settings > About Phone > Build Number
   - Verify Android version
   - Test cellular network, Wi-Fi, Bluetooth
   - Test camera, sensors
   - Install essential apps

## Step 8: Post-Installation

### Enable Developer Options

1. Go to Settings > About Phone
2. Tap "Build Number" 7 times
3. Developer Options now available

### Optional: Root with Magisk

If you flashed Magisk:
1. Open Magisk app
2. Grant root permissions to apps as needed
3. Install Magisk modules if desired

### Install Custom Kernel (Optional)

If you want to install a custom kernel:

1. Download kernel zip (e.g., from Coconutat's repo)
2. Boot to recovery
3. Flash kernel zip
4. Reboot

### Restore Your Data

1. Reinstall apps from Play Store
2. Restore backups
3. Import contacts
4. Copy photos/videos back

## Troubleshooting Common Issues

### Phone Won't Boot (Bootloop)

**Solutions**:
1. Boot to recovery
2. Wipe cache + dalvik cache
3. Reboot
4. If still fails: Factory reset and re-flash ROM

### "No Command" Screen

This means you're in stock recovery (not TWRP):
1. Hold **Power + Volume Up** together
2. Release when recovery menu appears
3. Or: Re-flash custom recovery

### Stuck at Mi Logo

**Solutions**:
1. Hold **Power** for 15 seconds to force restart
2. Try booting to recovery (Volume Up + Power)
3. Re-flash recovery and ROM

### Mobile Data Not Working

**Solutions**:
1. Settings > Network > APN Settings
2. Reset to default
3. Or manually enter carrier APN settings
4. Check if IMEI is present: Dial `*#06#`
5. If IMEI is 0 or missing: Restore EFS backup

### Camera Not Working

**Solutions**:
1. Install MIUI Camera port from XDA
2. Use GCam (Google Camera port)
3. Ensure camera permissions granted

### Wi-Fi Not Working

**Solutions**:
1. Settings > Wi-Fi > Advanced > Reset Wi-Fi
2. Reboot
3. Re-flash firmware base

### Fingerprint Not Working

**Solutions**:
1. Delete existing fingerprints
2. Re-register fingerprints
3. Ensure screen is clean
4. May need firmware update

## Reverting to Stock MIUI

If you want to go back to stock:

1. **Download Stock Fastboot ROM**:
   - From: https://mifirm.net/model/ruby.ttt
   - Get latest MIUI version for your region

2. **Download Mi Flash Tool**:
   - From: https://xiaomiflashtool.com/

3. **Flash Stock ROM**:
   - Extract ROM zip
   - Boot phone to Fastboot mode
   - Open Mi Flash Tool
   - Select ROM folder
   - Click "Flash"
   - Select "clean all" option
   - Click "Flash" and wait

4. **Re-lock Bootloader** (Optional):
   - **WARNING**: Only re-lock if on official ROM!
   - Re-locking with custom ROM will BRICK your device!
   ```bash
   fastboot oem lock
   ```

## Resources and Links

### Official Sources
- **LineageOS Builds**: https://github.com/bengris32/releases/releases
- **Xiaomi Kernel Source**: https://github.com/MiCode/Xiaomi_Kernel_OpenSource/tree/ruby-s-oss
- **Platform Tools**: https://developer.android.com/studio/releases/platform-tools

### Community Resources
- **XDA Forums (Ruby)**: https://xdaforums.com/f/redmi-note-12-pro-5g.12845/
- **Xiaomi.eu Community**: https://xiaomi.eu/community/
- **r/Xiaomi Reddit**: https://reddit.com/r/Xiaomi

### Download Sites
- **Firmware**: https://mifirm.net/model/ruby.ttt
- **ROMs**: https://xiaomirom.com/
- **TWRP**: https://xiaomitools.com/download/redmi-note-12-pro-plus-5g-ruby-twrp/

### Important XDA Threads
- TWRP and OrangeFox Recovery: https://xdaforums.com/t/recovery-twrp-and-orangefox-for-redmi-note-12-pro-5g-ruby.4668092/
- Custom ROMs Discussion: Search XDA Forums for "ruby custom rom"

## FAQ

**Q: Will I lose my data?**
A: Yes, all custom ROM installation requires wiping data.

**Q: Can I update LineageOS without wiping?**
A: Yes, you can "dirty flash" updates (flash new version without wiping data), but clean flash recommended for major updates.

**Q: Do I need to flash firmware before ROM?**
A: For LineageOS 21, yes - flash MIUI 14.0.6.0 TMOMIXM firmware first.

**Q: Will SafetyNet pass?**
A: Not by default. Use Magisk + Universal SafetyNet Fix module.

**Q: Can I use banking apps?**
A: Most will work. Some may require SafetyNet passing or hiding root.

**Q: Will OTA updates work?**
A: Yes, LineageOS and most custom ROMs support OTA updates.

**Q: Can I go back to MIUI?**
A: Yes, flash stock fastboot ROM using Mi Flash Tool.

**Q: Will warranty be void?**
A: Yes, unlocking bootloader voids warranty.

**Q: What about Widevine L1 (HD Netflix)?**
A: May downgrade to L3 after unlocking bootloader (no HD streaming).

## Final Notes

- Take your time and read everything carefully
- Join XDA Forums community for support
- Search before asking - most issues have been solved before
- Keep backups of working ROMs
- Don't panic if something goes wrong - most issues are fixable

**Good luck with your custom ROM installation!**

---
**Document Version**: 1.0
**Last Updated**: November 5, 2025
**Device**: Xiaomi Redmi Note 12 Pro 5G (ruby)
**Tested on**: Global variant (22101316G)
