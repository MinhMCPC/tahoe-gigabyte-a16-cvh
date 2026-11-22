# OpenCore EFI for GIGABYTE GAMING A16 CVH

[![OpenCore](https://img.shields.io/badge/OpenCore-1.0.x-blue.svg)](https://github.com/acidanthera/OpenCorePkg)
[![macOS](https://img.shields.io/badge/macOS-Tahoe%20%2F%20Sequoia%20%2F%20Sonoma-success.svg)](https://www.apple.com/macos)
[![SMBIOS](https://img.shields.io/badge/SMBIOS-MacBookPro16%2C2-orange.svg)](#smbios)
[![Status](https://img.shields.io/badge/Status-Daily%20Driver-brightgreen.svg)](#whats-working)

OpenCore EFI configuration for **GIGABYTE GAMING A16 CVH** laptop running macOS (Tahoe / Sequoia / Sonoma).

---

## 💻 Hardware Specifications

| Component | Details |
| :--- | :--- |
| **Model** | GIGABYTE GAMING A16 CVH |
| **CPU** | 13th Gen Intel® Core™ i7-13620H (10 Cores: 6 P-Cores + 4 E-Cores, 16 Threads, up to 4.90 GHz) |
| **RAM** | 16 GB DDR5 (Dual Channel) |
| **iGPU** | Intel® UHD Graphics (Raptor Lake GT1, Device ID: `8086:A7A8`) via **WhateverXe** |
| **dGPU** | NVIDIA® GeForce RTX™ 5060 Laptop GPU (Disabled in macOS - unsupported) |
| **Display** | 16" 16:10 FHD+ / QHD+ High Refresh Display |
| **Audio** | Realtek® ALC245 / ALC256 High Definition Audio + Intel Smart Sound |
| **Wi-Fi** | Intel® Wi-Fi 6E AX211 160MHz (`AirportItlwm.kext`) |
| **Bluetooth** | Intel® Bluetooth 5.3 (`IntelBluetoothFirmware` + `IntelBTPatcher` + `BlueToolFixup`) |
| **Ethernet** | Realtek® RTL8111 PCI-E Gigabit Ethernet (`RealtekRTL8111.kext`) |
| **Touchpad** | I2C HID Multi-touch Touchpad (`VoodooI2C` + `VoodooI2CHID`, polling mode) |
| **Keyboard** | PS/2 Keyboard with Fn Brightness control (`VoodooPS2Controller` + `BrightnessKeys`) |
| **Storage** | M.2 NVMe PCIe 4.0 SSD (`NVMeFix.kext`) |
| **Bootloader** | OpenCore 1.0.x |
| **SMBIOS** | `MacBookPro16,2` |

---

## ✅ What's Working & ❌ What's Not

### Working
- [x] **CPU Power Management**: Native speedstep and P/E core balancing via `CpuTopologyRebuild.kext` & `SSDT-PLUG-ALT.aml`.
- [x] **Graphics Acceleration**: Full Metal & hardware acceleration on Intel UHD Graphics via `WhateverXe.kext` (`-allow3d`, `-igfxdvmt`).
- [x] **Wi-Fi**: Working natively via `AirportItlwm.kext`.
- [x] **Bluetooth**: Working via `IntelBluetoothFirmware.kext` and `BlueToolFixup.kext`.
- [x] **Audio**: Speakers, headphone jack, and internal microphone.
- [x] **Ethernet**: Realtek RTL8111 Gigabit LAN.
- [x] **Keyboard**: Typing, media keys, and backlight.
- [x] **Screen Brightness**: Slider & Fn Hotkeys via `BrightnessKeys.kext` and `SSDT-PNLF.aml`.
- [x] **Touchpad**: Smooth cursor movement and multi-touch gestures via `VoodooI2C.kext` (`-vi2c-force-polling`).
- [x] **Battery**: Accurate battery percentage, charging state, and health via `SMCBatteryManager.kext`.
- [x] **USB Ports**: Mapped via `USBToolBox.kext` and `UTBDefault.kext`.
- [x] **Restart & Shutdown**.

### Not Working / Limitations
- [ ] **NVIDIA GeForce RTX 5060 Laptop GPU**: No macOS driver exists for modern NVIDIA architectures (Ampere/Ada Lovelace/Blackwell). The dGPU is inactive in macOS to conserve power and reduce heat.
- [-] **AirDrop / Continuity / Sidecar**: Limited support due to Intel Wi-Fi/BT card (AirDrop requires native Apple Broadcom hardware).
- [-] **Sleep / Wake**: Needs proper sleep mode configuration (`hibernatemode 0`) depending on connected external devices.

---

## ⚙️ Recommended BIOS Settings

Restart laptop and press `F2` or `Del` to enter BIOS Setup:

### Disable
- ❌ **Secure Boot**
- ❌ **Fast Boot**
- ❌ **Intel VMD / Intel RST** (Must be set to **AHCI**)
- ❌ **CFG Lock** (if option available in Advanced BIOS)

### Enable
- ✔️ **UEFI Boot Mode**
- ✔️ **Above 4G Decoding**
- ✔️ **Hyper-Threading**
- ✔️ **Intel Virtualization Technology (VT-x)**

---

## 🚀 Installation & SMBIOS Configuration

### ⚠️ IMPORTANT: Generate your own SMBIOS
Before booting with this EFI, you **MUST** generate your own unique SMBIOS serial numbers to avoid conflicts and enable Apple Services (iCloud, App Store, iMessage).

1. Download [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS).
2. Run `GenSMBIOS.bat` (Windows) or `GenSMBIOS.command` (macOS).
3. Select Option `1` to download MacSerial.
4. Select Option `2` and choose your `config.plist` (`EFI/OC/config.plist`).
5. Select Option `3` and enter:
   ```text
   MacBookPro16,2
   ```
6. Save the changes.

---

## 🧩 Key Kexts & ACPI Patches

### Kexts (`EFI/OC/Kexts`)
- **Lilu.kext** (v1.7.3) – Arbitrary kext and process patching engine.
- **WhateverXe.kext** (v0.8.0) – Graphics driver and framebuffers for 11th/12th/13th/14th Gen Intel Xe Graphics.
- **VirtualSMC.kext** (v1.3.9) – Advanced Apple SMC emulator with plugins:
  - `SMCBatteryManager.kext`
  - `SMCProcessor.kext`
  - `SMCLightSensor.kext`
  - `SMCSuperIO.kext`
- **CpuTopologyRebuild.kext** (v2.0.2) – Optimizes heterogeneous core scheduling (P-cores & E-cores).
- **AirportItlwm.kext** (v2.4.0) – Native Wi-Fi driver for Intel AX211.
- **IntelBluetoothFirmware.kext** (v2.5.0) & **BlueToolFixup.kext** (v2.7.3) – Intel Bluetooth support.
- **RealtekRTL8111.kext** (v2.4.2) – Realtek LAN driver.
- **VoodooI2C.kext** (v2.9.1) & **VoodooI2CHID.kext** – I2C Trackpad gestures.
- **VoodooPS2Controller.kext** (v2.3.8) – PS/2 Keyboard.
- **BrightnessKeys.kext** (v1.0.4) – Fn Brightness keys handler.
- **NVMeFix.kext** (v1.1.4) – Power management and stability for non-Apple NVMe.
- **AMFIPass.kext** (v1.4.1) – AMFI compatibility helper.
- **RestrictEvents.kext** (v1.1.7) – System information & update patches.
- **USBToolBox.kext** & **UTBDefault.kext** – Custom USB port mapping.

### ACPI (`EFI/OC/ACPI`)
- `DSDT.aml` – Base system DSDT patch.
- `SSDT-ALS0.aml` – Ambient Light Sensor stub.
- `SSDT-EC.aml` – Embedded Controller fix.
- `SSDT-GPI0.aml` – GPIO controller pin enablement for I2C trackpad.
- `SSDT-MCHC.aml` – Memory controller hub configuration.
- `SSDT-PLUG-ALT.aml` – Processor power management for Alder Lake / Raptor Lake.
- `SSDT-PNLF.aml` – Backlight control for laptop screen.
- `SSDT-RTCAWAC.aml` – Fix RTC / AWAC clock on modern Intel chipsets.
- `SSDT-SBUS.aml` – SMBus device fix.
- `SSDT-USB-Reset.aml` & `SSDT-USBX.aml` – USB power properties and reset.
- `SSDT-XOSI.aml` – OS vendor spoofing for Windows ACPI methods.

---

## 🛠️ Boot Arguments Reference

```text
-v debug=0x100 keepsyms=1 -allow3d -disablegfxfirmware -igfxdvmt -vi2c-force-polling ctrsmt=full -ibtcompatbeta revpatch=sbvmm nvfb=1 nvaccel=1 nvfbheads=4 -nvkmsnosmooth amfi_get_out_of_my_way=0x1 amfi=0x80 ipc_control_port_options=0
```

- `-allow3d`: Enables Metal 3D hardware acceleration in WhateverXe.
- `-disablegfxfirmware`: Bypasses non-existent Intel GFX firmware load.
- `-igfxdvmt`: Fixes DVMT pre-allocated memory requirement.
- `-vi2c-force-polling`: Prevents trackpad lag / interrupt freezes by forcing polling mode.
- `ctrsmt=full`: Core Topology Rebuild thread scheduler fix.
- `revpatch=sbvmm`: RestrictEvents patch for OTA updates and board ID compatibility.
- `amfi_get_out_of_my_way=0x1 amfi=0x80`: Bypasses AMFI restrictions for custom kexts and root patching.

---

## 🙏 Credits

- [Apple](https://www.apple.com) for macOS.
- [Acidanthera](https://github.com/acidanthera) for OpenCore, Lilu, VirtualSMC, WhateverGreen, AppleALC, VoodooPS2, and BrcmPatchRAM.
- [b00t0x](https://github.com/b00t0x) for WhateverXe (Intel Xe / Raptor Lake graphics support on macOS).
- [OpenIntelWireless](https://github.com/OpenIntelWireless) for itlwm / AirportItlwm and IntelBluetoothFirmware.
- [VoodooI2C](https://github.com/VoodooI2C/VoodooI2C) for I2C touchpad drivers.
- [MinhMCPC](https://github.com/MinhMCPC) for building, tuning, and testing this EFI.
