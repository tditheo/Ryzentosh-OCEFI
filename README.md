# EFI RYZENTOSH DESKTOP / LAPTOP
 
## FOR SETUP GUIDE, PLEASE GO TO THE [WIKI](https://github.com/tditheo/Ryzentosh-OCEFI/wiki)

## My Specifications
 
|  **Component**   | **Model**                                  |
| ---------------- | ------------------------------------------ |
| Processor        | AMD Ryzen 5 5500 @ 3.6GHz (4.2Ghz Turbo)   |
| Motherboard      | ASUS Prime A320M-C R2.0 (BIOS 6062)        |
| RAM              | 32GB (2 x 16GB) Corsair Vengeance @ 3600MHz|
| GPU              | MSI Radeon RX 6600 Mech X2                 |
| Audio Chipset    | ALC-887                                    |
| Ethernet         | Realtek RTL8111                            |
| SSD              | M.2 NVMe Samsung 980 1000GB PCIe 3.0	|

**macOS version**: 14.1 (23B74) \
**OpenCore version**: 1.0.0

### What's working :

- Ethernet
- GPU acceleration
- Audio
- Dual-booting with Windows (Set Kernel > Quirks > CustomSMBIOSGuid > to True (default is False) and PlatformInfo > UpdateSMBIOSMode > to Custom (default is Create) to avoid Activations issues).
- iMessage, FaceTime and others Apple Services

### What's not working :

- Microphone (due to AppleALC and AMD CPU)
- WiFi, Bluetooth, AirDrop, SideCar etc.. (because I don't have WiFi card)

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
