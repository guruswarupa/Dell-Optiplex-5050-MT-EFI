# Dell Optiplex 5050 MT Hackintosh EFI

## Hardware Specifications

**System:** Dell Optiplex 5050 MT (Mini Tower)
**CPU:** Intel Core i5-7500T (4th Generation Kaby Lake)
**GPU:** Intel HD Graphics 630 (Integrated)
**Ethernet:** Intel I210 Ethernet
**Audio:** Realtek Audio (Working)
**USB:** Custom USB configuration with tethering support

## Features

- ✅ **Graphics:** Intel HD 630 fully accelerated with HDMI/DisplayPort output
- ✅ **Audio:** Native audio support via AppleALC (layout ID 12)
- ✅ **Ethernet:** Intel I210 Ethernet working natively
- ✅ **USB:** All USB ports functional with tethering support
- ✅ **Sensors:** Dell-specific sensors (fan speed, temperatures)
- ✅ **Power Management:** Native power management with proper sleep/wake
- ✅ **NVMe:** NVMe SSD support with power management fixes

## SMBIOS Configuration

**Model:** iMacPro1,1
**Serial Number:** C02TGUZQHX87
**Board Serial Number:** C02712401GUJG36UE
**System UUID:** 73E56DF4-ED78-4326-8FFF-0647842FB94F

## Boot Arguments

```
-v alcid=12 watchdog=0 igfxonln=1
```

- `-v` - Verbose mode for debugging
- `alcid=12` - Audio layout ID for Dell Optiplex 5050
- `watchdog=0` - Disable watchdog timer
- `igfxonln=1` - Force Intel graphics to be treated as built-in

## Kexts (Kernel Extensions)

### Essential Kexts
- **Lilu.kext** - Essential patching engine
- **VirtualSMC.kext** - SMC emulation and sensors
- **WhateverGreen.kext** - Intel graphics patches and acceleration

### Audio Kexts
- **AppleALC.kext** - Native audio support for Realtek codecs
- **VoodooHDA.kext** - Alternative audio driver (backup)

### Network Kexts
- **AppleIntelI210Ethernet.kext** - Intel I210 Ethernet support
- **IntelMausi.kext** - Intel Ethernet support
- **AtherosE2200Ethernet.kext** - Atheros Ethernet support
- **RealtekRTL8111.kext** - Realtek Ethernet support
- **LucyRTL8125Ethernet.kext** - Realtek 2.5G Ethernet support

### USB Kexts
- **USBInjectAll.kext** - USB port injection and mapping
- **XHCI-unsupported.kext** - XHCI (USB 3.0) support for older systems

### System Kexts
- **NVMeFix.kext** - NVMe power management and compatibility fixes
- **RestrictEvents.kext** - Restrict incompatible events and extensions
- **AirportBrcmFixup.kext** - Broadcom WiFi support (if applicable)

### SMC Sensor Plugins
- **SMCProcessor.kext** - CPU power management and temperature
- **SMCSuperIO.kext** - SuperIO sensor support
- **SMCDellSensors.kext** - Dell-specific sensor support
- **SMCLightSensor.kext** - Ambient light sensor support
- **SMCBatteryManager.kext** - Battery management (if applicable)
- **SMCRadeonGPU.kext** - GPU power management
- **RadeonSensor.kext** - GPU temperature monitoring

## ACPI (Advanced Configuration and Power Interface)

### Custom SSDTs
- **SSDT-EC-USBX.aml** - Embedded Controller and USB power management
- **SSDT-GPRW.aml** - GPRW fix for proper wake from sleep
- **SSDT-HPET.aml** - HPET fix for timer compatibility
- **SSDT-OLARILA.aml** - OLARILA-specific power management fixes
- **SSDT-PLUG.aml** - CPU power management plug-in
- **SSDT-PMCR.aml** - Power management control register fix
- **SSDT-S3-BLOCK.aml** - Block S3 sleep (uses S3 if supported)

## Graphics Configuration

**Framebuffer Patching:** Enabled
**Platform ID:** 0x59120003 (Intel HD 630 mobile)
**Video Memory:** 2048 MB (patched)
**Ports:** 3 DisplayPort/HDMI ports configured
**HDMI Audio:** Enabled via hda-gfx injection

## DeviceProperties

### Intel Graphics Device (PciRoot(0x0)/Pci(0x2,0x0))
- `AAPL,ig-platform-id`: 0x00000300 (Mobile HD 630)
- `enable-hdmi20`: Enabled
- `enable-lspcon-support`: Enabled (HDMI/DP converters)
- `framebuffer-patch-enable`: Enabled
- `hda-gfx`: onboard-1 (for HDMI audio)

## OpenCore Configuration

**OpenCore Version:** 0.9.7+ (based on file dates)
**Drivers:**
- HfsPlus.efi - HFS+ filesystem support
- OpenCanopy.efi - GUI bootloader theme
- OpenRuntime.efi - OpenCore runtime services
- ResetNvramEntry.efi - NVRAM reset utility

**Security:** 
- Secure Boot: Disabled
- DmgLoading: Signed only
- ScanPolicy: 0 (scan all devices)

**Boot:**
- Timeout: 5 seconds
- Picker Mode: External
- Show Picker: Enabled
- Hide Auxiliary: Disabled

## Important Notes

1. **Serial Numbers:** The SMBIOS serial numbers are unique to this system. When using this EFI on another system, you must generate new serial numbers using tools like GenSMBIOS.

2. **Audio Layout:** The audio layout ID (12) is specific to Dell Optiplex 5050. If audio doesn't work, you may need to try different layout IDs.

3. **Ethernet:** Multiple ethernet kexts are included for compatibility. Only the necessary one (Intel I210 for this system) is actively used.

4. **USB Tethering:** USB tethering works thanks to the custom USBInjectAll configuration and XHCI-unsupported kext.

5. **Sleep/Wake:** Sleep and wake functionality is implemented through SSDT-GPRW and proper power management configuration.

6. **Updates:** When updating macOS, ensure your kexts are compatible with the new version. The current configuration supports up to macOS Sonoma.

## Installation Instructions

1. **Prepare USB Installer:**
   - Create a macOS USB installer using official methods
   - Mount the EFI partition of the USB drive
   - Copy the entire EFI folder to the USB's EFI partition

2. **Install macOS:**
   - Boot from the USB drive using OpenCore
   - Install macOS normally
   - During installation, use boot flags if needed: `-v alcid=12`

3. **Post-Installation:**
   - Mount your system drive's EFI partition
   - Copy the EFI folder to your system drive's EFI partition
   - Remove the USB installer and boot from your system drive

4. **Configuration:**
   - Update SMBIOS information for your specific system
   - Test all hardware components
   - Configure any additional settings as needed

## Troubleshooting

### No Audio Output
- Try different audio layout IDs (common ones: 1, 11, 12, 13, 28, 29)
- Check that VoodooHDA is not conflicting with AppleALC
- Verify audio device in System Preferences > Sound

### Graphics Issues
- Ensure WhateverGreen.kext is properly loaded
- Try removing `igfxonln=1` if encountering graphics issues
- Check framebuffer patching configuration

### Sleep/Wake Issues
- Verify SSDT-GPRW is properly configured
- Check that all USB ports have proper power management
- Ensure watchdog timer is disabled (watchdog=0)

### USB Tethering Not Working
- Verify USBInjectAll.kext is loaded
- Check that XHCI-unsupported.kext is properly installed
- Ensure USB ports are correctly mapped

## Credits

This EFI configuration is based on community research and testing:
- Acidanthera for OpenCore and essential kexts
- Dortania for comprehensive Hackintosh guides
- OLARILA for ACPI patches and power management fixes
- Community members who contributed to Dell Optiplex support

## Disclaimer

This EFI configuration is provided as-is for educational purposes. Hackintoshing may violate Apple's End User License Agreement (EULA). Use at your own risk. Always backup your data before modifying system files.

## Compatibility

- **macOS Versions:** Sierra (10.12) through Sonoma (14.x)
- **OpenCore:** 0.9.7+
- **Hardware:** Dell Optiplex 5050 MT with i5-7500T specifically

## Support

For issues and questions:
- Check Dortania's debugging guide
- Visit r/hackintosh on Reddit
- Search Olarila and InsanelyMac forums
- Review OpenCore documentation

---

**Last Updated:** July 2025
**Tested Configuration:** macOS Sonoma 14.x
**Status:** Stable - All major features working