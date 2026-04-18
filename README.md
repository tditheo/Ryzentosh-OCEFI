# EFI RYZENTOSH DESKTOP / LAPTOP
 
## FOR THE SETUP GUIDE, PLEASE CHECK OUT THE [WIKI](https://github.com/tditheo/Ryzentosh-OCEFI/wiki)

## My Specifications
 
|  **Component**   | **Model**                                  |
| ---------------- | ------------------------------------------ |
| Processor        | AMD Ryzen 5 5500 @ 3.6GHz (4.2Ghz Turbo)   |
| Motherboard      | ASUS ROG Strix B550-A Gaming (BIOS 3636)   |
| RAM              | 32GB (2 x 16GB) Corsair Vengeance @ 3600MHz|
| GPU              | MSI Radeon RX 6600 Mech X2                 |
| Audio Chipset    | ALC-S1200A                                 |
| Ethernet         | Intel i225-V 2.5Gb Ethernet                |
| SSD              | M.2 NVMe Samsung 980 1000GB PCIe 3.0	|

**macOS version**: 26.4.1 (25E253) \
**OpenCore version**: 1.0.7

### What's working :

- Ethernet
- GPU acceleration
- Audio
- Dual-booting with Windows (Set Kernel > Quirks > CustomSMBIOSGuid > to True (default is False) and PlatformInfo > UpdateSMBIOSMode > to Custom (default is Create) to avoid Activations issues).
- iMessage, FaceTime and others Apple Services

### What's not working :

- 3.5mm jack Microphone (due to AppleALC and AMD CPU)
- WiFi, Bluetooth, AirDrop, SideCar etc.. (because I don't have a WiFi card)
- AppleALC in macOS Tahoe (USB Audio works perfectly) [See more here](https://dortania.github.io/OpenCore-Install-Guide/extras/tahoe.html)

## Credits

- [Apple](https://apple.com) for macOS
- [AMD-OSX Developers](https://github.com/AMD-OSX) for kernel patches for AMD CPUs
- [Acidanthera](https://github.com/acidanthera) for OpenCore and most of used kexts
- [Trulyspinach](https://github.com/trulyspinach) for Ryzen power management and monitoring kexts
- [DhinakG](https://github.com/USBToolBox) for USBToolBox
- [XLNC](https://github.com/naveenkrdy) for AppleMCEReportedDisabler kext
- [Pavo](https://github.com/Pavo-IM) for research about AppleMCEReportedDisabler in Monterey
- [Dortania](https://github.com/dortania) for OpenCore configuration guides
- [ChefKissInc](https://github.com/ChefKissInc) (and his telegram group) for getting AMD APU working on macOS 
