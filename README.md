# EFI RYZENTOSH DESKTOP / LAPTOP
 
 
 
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

- [macOS Version Compatibility](#macOS-Version-Compatibility)
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

<sup>1</sup>More information : [Dortania's GPU Buyers Guide](https://dortania.github.io/GPU-Buyers-Guide/modern-gpus/amd-gpu.html) \
Older versions of macOS are not covered here.

## Hardware Compatibility

### Processor (CPU)

This EFI is compatible with all Ryzen.

### Graphical Processing Unit (GPU)

| **Model**  | **Compatible?**               |
| ---------- | ----------------------------- |
| Integrated | (Almost) Yes, using [NootedRed](https://github.com/NootInc/NootedRed)|
| Nvidia     | Partially <sup>1</sup>        |
| AMD        | Yes <sup>2</sup> |

<sup>1</sup> NVidia GT(x) 600 / 700 was dropped in macOS 12 (Monterey), but you can revert this thanks to [OpenCore Legacy Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher). Other NVidia GPU are not supported.

<sup>2</sup> All Polaris / Vega / Navi GPU are supported in last macOS Version. Older GPUs can be not compatible or need FakeID (More information : [GPU Buyers Guide](https://dortania.github.io/GPU-Buyers-Guide/modern-gpus/amd-gpu.html#r7-r9)

For **AMD Navi 10 / 21 / 23 Series GPUs (RX 5000 / 6000)** you need to add `agdpmod=pikera` to `boot-args` to fix the black screen issue.

[PAT Patch made by Shaneee](#PAT-Patch) is used by default. It improves GPU performance but it has a few caveats. Audio passed by HDMI or DisplayPort won't work or will be unstable. It also **may not** work with Nvidia GPUs.

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
| AMD Navi Series      | MacPro7,1               | 
| AMD Vega Series      | MacPro7,1               |
| AMD Polaris Series   | MacPro7,1               |
| AMD Radeon R5/R7/R9  | MacPro6,1               |
| AMD HD 8000 Series   | MacPro6,1               |
| AMD HD 7000 Series   | MacPro6,1               |
| Nvidia Kepler Series | MacPro7,1               |

### Configuration

5. Map your USB ports with [USBToolBox](https://github.com/USBToolBox/tool). Guide about it is available [here](https://github.com/USBToolBox/tool#usage). You have to do it from Windows.

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

<sup>1</sup> If you have this option in BIOS you must also add `npci=0x2000` from `boot-args` in your configuration file.

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
