# Hackintosh ASUS ROG STRIX B660I

**Latest working macOS**: MacOS Ventura 13.0.1
<br>
**Current OpenCore**: 0.8.5-DEGUB

## Complete hardware specs
- Intel i5 12400f
- ASUS ROG STRIX B660I (BIOS Ver. 2012)
- Radeon RX6600XT
- 32Gb DDR5 4800Mhz
- Kioxia EXCERA G2 1TB NVMe
- Wifi/BT Fenvi bcm94360ng (replaced with preinstalled intel AX201NGW)

## What works
- macOS Ventura
- HDMI/DP
- Wifi/BT
- Shutdown

## What doesn't work

## Not tested
- Audio
- Sleep
- Reboot

## ToDo
- add Intel I225-V LAN [reference](https://github.com/5T33Z0/Gigabyte-Z490-Vision-G-Hackintosh-OpenCore/blob/main/I225-V_FIX.md#option-1-using-a-ssdt-with-corrected-header-description) or try [tricking Apple's I225LM driver into supporting our I225-V](https://dortania.github.io/OpenCore-Install-Guide/config.plist/comet-lake.html#deviceproperties)
- USB Mapping

## Kexts used:
- [Lilu.kext](https://github.com/acidanthera/Lilu/releases)  
-- must have to boot
- [VirtualSMC.kext](https://github.com/acidanthera/VirtualSMC/releases)  
-- must have to boot
- [WhateverGreen](https://github.com/acidanthera/WhateverGreen/releases)  
-- for graphics
- [AppleALC.kext](https://github.com/acidanthera/AppleALC/releases)  
-- for audio

## BIOS settings
- Advanced > CPU Configuration > Intel (VMX) Virtualization Technology > Enabled (defalut)
- Advanced > CPU Configuration > Active Performance Cores > All (i5 12400f have no E-Cores, unknown)
- Advanced > CPU Configuration > Hyper-Threading > Enabled (defalut)
- Advanced > System Agent (SA) Configuration > VT-d > Enabled (defalut)
- Advanced > System Agent (SA) Configuration > Control Iommu Pre-boot Behavior > Disable IOMMU (default? not sure)
- Advanced > PCI Subsystem Settings > Above 4G Decording > Enabled (defalut)
- Advanced > PCI Subsystem Settings > Re-Size BAR Support > Enabled (defalut)
- Advanced > USB Configuration > Legacy USB Support > Enabled
- Advanced > USB Configuration > XHCI Hand-off > Enabled
- Advanced > Network Stack Configuration > Network Stack > Disabled
- Advanced > USB Configuration > Legacy USB Support > Enabled
- Boot > CSM (Compatibility Support Module) > Launch CSM > Disabled
- Boot > Secure Boot > OS Type > Other OS
- Boot > Secure Boot Mode > Custom
- Boot > Boot Configuration > Fast Boot > Disabled

## To use this EFI
- I deleted information about Serial, Board Serial, SmUUID, and Apple ROM. If you use this EFI, please use GenSMBIOS and generate required information.
- [SMBIOS](https://github.com/corpnewt/GenSMBIOS)
- [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/config.plist/#creating-your-config-plist)

## Thanks/Credits
- [Opencore Team](https://dortania.github.io/getting-started/)
