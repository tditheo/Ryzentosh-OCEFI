# EFI config. for Ryzen 5 5500
 
 
 
## My Specifications
 
|  **Component**   | **Model**                                  |
| ---------------- | ------------------------------------------ |
| Processor        | AMD Ryzen 5 5500 @ 3.6GHz (4.2Ghz Turbo)   |
| Motherboard      | ASUS Prime A320M-C R2.0 (BIOS 6062)        |
| RAM              | 32GB (2 x 16GB) Corsair Vengeance @ 3600MHz|
| GPU              | MSI Radeon RX 6600 Mech X2                 |
| Audio Chipset    | ALC-887                                    |
| Ethernet         | Realtek RTL8111                            |
| SSD              | M.2 NVMe PNY CS1030 500GB PCIe 3.0	     |

**macOS version**: 13.3.1 (22E261) \
**OpenCore version**: 0.9.1

## Table of contents

- [Software Compatibility](#macOS-Version-Compatibility)
- [Hardware Compatibility](#Hardware-Compatibility)
- [Installation](#Installation)
- [BIOS Settings](#BIOS-Settings)
- [PAT Patch](#PAT-Patch)
- [Sleep](#Sleep)
- [Guides](#Guides)
- [Credits](#Credits)

## macOS Version Compatibility

- Ventura (13.x) (Works)
- Monterey (12.x) (NOT TESTED / Supposed to works)
- Big Sur (11.x) (NOT TESTED / Navi 23 NOT COMPATIBLE<sup>1</sup>)
- Catalina (10.15.x) (NOT TESTED / Navi 23 / 21 NOT COMPATIBLE<sup>1</sup><sup>2</sup>)
- Mojave (10.14.x) (NOT TESTED / Navi 23 / 21 / 10 NOT COMPATIBLE<sup>1</sup><sup>2</sup>)
- High Sierra (10.13.x) (NOT TESTED / GPU NOT COMPATIBLE<sup>1</sup><sup>2</sup>)

<sup>1</sup>More information : [Dortania's GPU Buyers Guide](https://dortania.github.io/GPU-Buyers-Guide/modern-gpus/amd-gpu.html)
<sup>2</sup>These versions of macOS are not covered here

## Hardware Compatibility

### Processor (CPU)
0000000
This EFI is compatible with all Ryzen and Athlon 2xxGE processors with
[macOS-compatible peripherals](https://dortania.github.io/Anti-Hackintosh-Buyers-Guide/).

_Support for 15h (FX series), 16h (A series) and Threadripper CPUs is not covered here._

### Graphical Processing Unit (GPU)

| **Model**  | **Compatible?**               |
| ---------- | ----------------------------- |
| Integrated | (Almost) Yes, using [NootedRed](https://github.com/NootInc/NootedRed)|
| Nvidia     | Partially <sup>1</sup>        |
| AMD        | Yes <sup>2</sup> |

<sup>1</sup> NVidia GT(x) 600 / 700 was dropped in macOS 12 (Monterey), but you can revert this thanks to [OpenCore Legacy Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher). Kepler series under correct [PAT Patch](#PAT-Patch). Others require WebDrivers which work only in High Sierra or are not supported. More details on [Dortania](https://dortania.github.io/GPU-Buyers-Guide/modern-gpus/nvidia-gpu.html).

<sup>2</sup> All Polaris / Vega / Navi GPU are supported in last macOS Version. Older GPUs can be not compatible or need FakeID (More information : [GPU Buyers Guide](https://dortania.github.io/GPU-Buyers-Guide/modern-gpus/amd-gpu.html#r7-r9)

For **AMD Navi 10 / 21 / 23 Series GPUs (RX 5000 / 6000)** you need to add `agdpmod=pikera` to `boot-args` to fix the black screen issue.

[PAT Patch made by Shaneee](#PAT-Patch) is used by default. It improves GPU performance but it has a few caveats. Audio passed by HDMI or DisplayPort won't work or will be unstable. It also **may not** work with Nvidia GPUs.

If you want to control monitor's brightness or HDMI/DP audio volume you need to use [MonitorControl](https://github.com/MonitorControl/MonitorControl) for that.

### Laptops

Latops with Ryzen 3000 or below **may be** compatible using [NootedRed](https://github.com/NootInc/NootedRed)|)

### Motherboards

| **Chipset**                  | **Details**                                                         |
| ---------------------------- | ------------------------------------------------------------------- |
| B550, A520                   | Requires _SSDT-CPUR_ to boot. [Details here.](#SSDT-CPUR)           |
| B550, A520, B450, X470, X570 | `SetupVirtualMap` has to be [disabled](#Disabling-SetupVirtualMap). |
| Other                        | Should be compatible out of the box.                                |

#### _SSDT-CPUR_

Follow these steps to properly install _SSDT-CPUR_.

- Download from [here](https://github.com/naveenkrdy/Misc/blob/master/SSDTs/Compiled/SSDT-CPUR.aml).
- Install it to your `OC/ACPI` directory.
- Add it to your configuration file. Use ProperTree for simplicity.

#### _Disabling SetupVirtualMap_

To disable `SetupVirtualMap` simply go to `Booter -> Quirks -> SetupVirtualMap` in your configuration file and disable it. (Should be `false`).

### Audio

Follow these steps if your audio chipset is different than the one specified in the [Specification](#My-Specifications).

- Go [here](https://github.com/acidanthera/applealc/wiki/supported-codecs) to find your audio chipset codec. (Under _Codec_)
  - If you can't find your codec on the list, then you probably have to use [VoodooHDA](https://sourceforge.net/projects/voodoohda/). This repository does not cover or support the use of VoodooHDA.
- Under _Revisions and layouts_ you'll see bunch of numbers and layout ids.
- Find your `boot-args` settings and look for `alcid=11`.
- Try every layout id (except _0x_ values) one by one until it works.
  - Example: `alcid=10` if `layout 10`

_Caveats_:

- External (USB) audio cards should work out of the box.
- Microphone input (3.5mm Jack) won't work properly. Use USB/Bluetooth microphones instead.
- If you have CPU with integrated GPU (like Ryzen 3 3200G) your audio may be broken or unstable. You can try using [SpeedKeeper](https://github.com/astrihale/speedkeeper) but it's not guaranteed to fix your problem. The best solution is using external (USB) audio card.

### Network

If you experience any issues with your network connection, then your best bet would be to install a different kext, preferably from [here](https://dortania.github.io/OpenCore-Install-Guide/ktext.html#ethernet).

If you use High Sierra and Realtek 8111 Ethernet Card then you should use [older version of kext](https://github.com/Mieze/RTL8111_driver_for_OS_X/releases/tag/v2.2.2).

If you have issues with network card on Monterey or newer try adding `e1000=0` to `boots-args`.

SmallTree kext does not work on Monterey for now. You can try [AppleIGB kext](https://cdn.discordapp.com/attachments/724618275971137568/879288441278435348/AppleIGB.kext.zip), it works on some systems. If it does not work you have to stay on Big Sur and wait for SmallTree's update.

### WiFi and Bluetooth

Support for WiFi and Bluetooth is not covered here. You can check on Dortania's Guide to find compatible network card.
## Installation

### Bootable USB

1. Follow [this guide](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/) to create your bootable USB.

2. Download this repository and copy "BOOT" & "OC" directories to your "EFI" directory on your bootable USB. The structure should look somewhat like this: `EFI -> BOOT, OC`.

### Modifying kernel patches
3. Modify Core Count patches to match your CPU's cores amount.

  - Find four `algrey - Force cpuid_cores_per_package` patches under `Kernel -> Patch` in your config.
  - Modify these patches for your CPU physical cores. Change **first pair** of `00` in `Replace` of these patches to `Hex value` from below table.

    - e. g. for Ryzen 5 5500 with 6 Cores three modified patches should look like:
      - B8 **00** 0000 0000 -> B8 **06** 0000 0000
      - BA **00** 0000 0000 -> BA **06** 0000 0000
      - BA **00** 0000 0090 -> BA **06** 0000 0090
      - BA **00** 0000 00 -> BA **06** 0000 00

| **Physical CPU cores** | **Hex value** |
| ---------------------- | ------------- |
| 4 Cores                | `04`          |
| 6 Cores                | `06`          |
| 8 Cores                | `08`          |
| 12 Cores               | `0C`          |
| 16 Cores               | `10`          |
| 24 Cores               | `18`          |
| 32 Cores               | `20`          |

### SMBIOS

**You MUST use your SMBIOS informations to have working Apple Services. There is no SMBIOS info in this EFI Config**

4. Use [this tool](https://github.com/corpnewt/GenSMBIOS) to generate your unique SMBIOS info.

- SMBIOS has to be unique, you cannot use one present in this repository.

- Run GenSMBIOS and select `Download MacSerial` and `Generate SMBIOS`.
- Select the appropriate model for your hardware using the table below.
- Go to [Apple Coverage](https://checkcoverage.apple.com/) and paste generated _Serial_. You need "Invalid Serial" or "Purchase Date not Validated" message.
- Open _config.plist_ and search for `PlatformInfo -> Generic` and replace these values:
  - _SystemProductName_ -> Model
  - _MLB_ -> Board Serial
  - _SystemSerialNumber_ -> Serial
  - _SystemUUID_ -> SmUUID
  - _ROM_ -> ROM

| **GPU Series**       | **Model**               |
| -------------------- | ----------------------- |
| AMD Navi Series      | MacPro7,1 <sup>1</sup> |
| AMD Vega Series      | MacPro7,1 <sup>1</sup> |
| AMD Polaris Series   | MacPro7,1 <sup>1</sup> |
| AMD Radeon R5/R7/R9  | MacPro6,1               |
| AMD HD 8000 Series   | MacPro6,1               |
| AMD HD 7000 Series   | MacPro6,1               |
| Nvidia Kepler Series | MacPro7,1 <sup>2</sup>  |

<sup>1</sup> For Catalina and older, you can use iMacPro1,1 tu avoid any issues.

<sup>2</sup> For Catalina and older use `iMac14,2`.

### Configuration

5. You should update your BIOS to the latest version and configure it appropriately. See [BIOS Settings](#BIOS-Settings) for details.
6. Remember to verify your hardware and apply appropriate changes to your configuration file. See [Hardware Compatibility](#Hardware-compatibility) for details.
7. Map your USB ports with [USBToolBox](https://github.com/USBToolBox/tool). Guide about it is available [here](https://github.com/USBToolBox/tool#usage). You have to do it from Windows.
8. That's it! Now you can boot macOS installer.

## BIOS Settings

| **Option**            | **Status**           |
| --------------------- | -------------------- |
| SATA Mode             | AHCI                 |
| Above 4G Decoding     | Enabled <sup>1</sup> |
| EHCI/XHCI Hand-off    | Enabled              |
| SVM                   | Enabled              |
| CSM                   | Disabled             |
| Resizable BAR Support | Disabled             |
| Secure Boot           | Disabled             |
| Serial Port           | Disabled             |
| Parallel Port         | Disabled             |

<sup>1</sup> If you have this option in BIOS you must also remove `npci=0x2000` from `boot-args` in your configuration file.

**Some of these options may not exist in your firmware, just try to match it as closely as possible.**

## PAT Patch

| **Shaneee's**                 | **Algrey's**             |
| ----------------------------- | ------------------------ |
| Much better GPU performance   | Worse GPU performance    |
| May not work with Nvidia GPUs | Compatible with all GPUs |
| HDMI/DP audio may not work    | HDMI/DP audio works      |
| Enabled by default            | Disabled by default      |

To switch to another patch look for `mtrr_update_action` in `config.plist`. Then set `Enabled` to `true` for the patch you want to use. Remember to set `Enabled` to `false` on the other PAT patch. Do not try to enable both at the same time, trust me, it won't work.

## Sleep
Firstly, check if your sleep works out of the box. If it works, you can skip reading this section.

The most common reason of broken sleep on AMD systems are USB problems. \
You have to map your USB ports. If you have working Windows instance I recommend [this tool](https://github.com/usbtoolbox/tool), otherwise you have to [do it manually](https://dortania.github.io/OpenCore-Post-Install/usb/#macos-and-the-15-port-limit). \
After mapping remember to disable `Kernel -> Quriks -> XhciPortLimit` in your configuration file.

If map does not fix your issue you can try to patch `_STA` method of your USB controllers. Go to `ACPI -> Add` in your configuration file and set `Enabled` to `true` under `SSTD-SLEEP` section. By default this SSDT patches `_SB.PCI0.GPP2.PTXH` device. In most cases your controller will have another address. \
You can find it with [Hackintool](https://github.com/headkaze/Hackintool) - go to `PCIe` tab, then look for devices with `USB controller` value under `Subclass`. When you find address of your controller you have to open `SSDT-SLEEP.aml` with [MaciASL](https://github.com/acidanthera/MaciASL) and change addresses.
If you have more than one controller you can try to patch `_STA` for every of them.

If USB fixes does not help, probably something another is broken. You can read more detailed guide about it on [Dortania](https://dortania.github.io/OpenCore-Post-Install/universal/sleep.html).

## Guides

- Creating USB installer: [\*click\*](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/)
- OpenCore configuration: [\*click\*](https://dortania.github.io/OpenCore-Install-Guide/AMD/zen.html)
- Post-Install: [\*click\*](https://dortania.github.io/OpenCore-Post-Install/)
- Troubleshooting: [\*click\*](https://dortania.github.io/OpenCore-Install-Guide/troubleshooting/troubleshooting.html)
- ACPI patching: [\*click\*](https://dortania.github.io/Getting-Started-With-ACPI/)

## Credits

- [Apple](https://apple.com) for macOS
- [AMD-OSX Developers](https://github.com/AMD-OSX) for kernel patches for AMD CPUs
- [Acidanthera](https://github.com/acidanthera) for OpenCore and most of used kexts
- [Trulyspinach](https://github.com/trulyspinach) for Ryzen power management and monitoring kexts
- [Mieze](https://github.com/Mieze) for RealtekRTL8111 kext
- [DhinakG](https://github.com/USBToolBox) for USBToolBox
- [XLNC](https://github.com/naveenkrdy) for AppleMCEReportedDisabler kext
- [Pavo](https://github.com/Pavo-IM) for research about AppleMCEReportedDisabler in Monterey
- [Dortania](https://github.com/dortania) for OpenCore configuration guides
- [NootInc](https://github.com/NootInc) (and his telegram group) for got AMD APU working on macOS 
