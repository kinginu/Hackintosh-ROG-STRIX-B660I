# Hackintosh ASUS ROG Strix B660-I
Opencore Hackintosh settings for ASUS ROG Strix B660-I Gaming WiFi

**Latest working macOS**: macOS Ventura 13.4  
**Current OpenCore**: 0.8.7 (RELEASE)

## Hardware
- **Motherboard**: ASUS ROG STRIX B660I (BIOS Ver. 2012)
- **CPU**: Intel i5 12400f
- **GPU**: Radeon RX6600XT
- **RAM**: Crucial DDR5 4800Mhz 32GB (2x16GB)
- **1st SSD**: Monster Storage 2TB NVMe SSD PCIe Gen 4×4
- **2nd SSD**: Kioxia EXCERA G2 1TB NVMe SSD PCIe Gen 3×4
- **Wifi/BT**: Fenvi BCM94360NG (replaced with preinstalled intel AX201NGW)
- **PSU**: Corsair SF600 PLATINUM

## What Works
- macOS Ventura
- Shutdown / Reboot / Sleep / Wake
- HDMI / DP
- Ethernet
- Wifi / Bluetooth
- USB Ports
- Audio
- AirDrop / Handoff / Unlock with Apple Watch... (All Continuity features except Sidecar)
- DRM

## What Doesn't Work
- Sidecar

## Issues and Fixes
**Issue**: DisplayPort (DP) hotplug was not working properly, causing the display to occasionally not be recognized when plugging or unplugging the DP cable. Additionally, there were occasional crashes while using Zoom or other full-screen apps.

**Symptoms**: 
- The monitor would sometimes not display anything when connected via DP, or it would lose the signal when the cable was replugged.
- The system would occasionally crash when using Zoom or other full-screen applications.

**Solution**: Both issues were identified to be caused by the monitor's AMD FreeSync™ feature. Disabling AMD FreeSync on the monitor resolved both the DP hotplug issue and the full-screen app crashes. If you encounter similar problems, try turning off AMD FreeSync in your monitor's settings.

## SSDTs
|  | Notes |
| --- | --- |
| [SSDT-AWAC-DISABLE](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/AcpiSamples/Source/SSDT-AWAC-DISABLE.dsl) | Disable AWAC, enable RTC |
| [SSDT-EC-USBX](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/AcpiSamples/Source/SSDT-EC-USBX.dsl) | AppleUsbPower compatibility table |
| [SSDT-I225V](https://github.com/5T33Z0/OC-Little-Translated/tree/main/01_Adding_missing_Devices_and_enabling_Features) | Intel I225-V Ethernet Controller ([Reference](https://github.com/5T33Z0/Gigabyte-Z490-Vision-G-Hackintosh-OpenCore/blob/main/I225-V_FIX.md#option-1-using-a-ssdt-with-corrected-header-description)) |
| [SSDT-PLUG-ALT](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/AcpiSamples/Source/SSDT-PLUG-ALT.dsl) | XCPM power management compatibility table |

## Kexts
|  | Ver | Notes |
| --- | --- | --- |
| [AppleALC](https://github.com/acidanthera/AppleALC) | 1.7.6 | AppleHDA patching, support for most on-board sound controllers |
| AppleIntelI210Ethernet | - |  Intel I225-V Ethernet Controller support ([Reference](https://github.com/5T33Z0/Gigabyte-Z490-Vision-G-Hackintosh-OpenCore/blob/main/I225-V_FIX.md#option-1-using-a-ssdt-with-corrected-header-description)) |
| [Lilu](https://github.com/acidanthera/Lilu) | 1.6.2 | Patching many processes |
| [NVMeFix](https://github.com/acidanthera/NVMeFix) | 1.1.0 | Fixing power management and initialization on non-Apple NVMe |
| [RadeonSensor](https://github.com/aluveitie/RadeonSensor) | 0.3.1 | Provide GPU temperature |
| [RestrictEvents](https://github.com/acidanthera/RestrictEvents) | 1.0.9 | [Fixing MacPro7,1 Memory Errors](https://dortania.github.io/OpenCore-Post-Install/universal/memory.html) |
| [SMCProcessor](https://github.com/acidanthera/VirtualSMC) | 1.3.0 | Provide Intel CPU temperature |
| [SMCRadeonGPU](https://github.com/aluveitie/RadeonSensor) | 0.3.1 | Provide GPU temperature |
| [SMCSuperIO](https://github.com/acidanthera/VirtualSMC) | 1.3.0 | Provide fan speed |
| USBPorts | - | - |
| [VirtualSMC](https://github.com/acidanthera/VirtualSMC) | 1.3.0 | Emulates the SMC chip found on real Macs |
| [WhateverGreen](https://github.com/acidanthera/WhateverGreen) | 1.6.2 | Graphics patching, DRM fixes, board ID checks, framebuffer fixes |

- Previously used [USBInjectAll.kext from daliansky](https://github.com/daliansky/OS-X-USB-Inject-All/releases) for USB port mapping. Now using USBPorts.kext. Note that the [Original USBInjectall (from Sniki)](https://github.com/Sniki/OS-X-USB-Inject-All) does not work with Alder Lake chipset.

## config.plist Setup
- First open ProperTree and take Snapshot.
- Changes from sample.plist is shown below.

### ACPI
- Nothing to do.

### Booter>Quirks
| key | | Comment |
| --- | --- | --- |
| DevirtualizeMmio | true |
| EnableWriteUnprotector | false |
| ProtectUefiServices | true | |
| RebuildAppleMemoryMap | true | |
| ResizeAppleGpuBars | 0 | Set to -1 if Resizable BAR is disabled in BIOS setting |
| SetupVirtualMap | false | |
| SyncRuntimePermissions | true | |

### DeviceProperties>Add
- PciRoot(0x0)/Pci(0x1F,0x3)  

| key | Type | Value | Comment |
| --- | --- | --- | --- |
| layout-id | Data | 0D000000 | Applies AppleALC audio injection (layout-id=13) |

```
	<key>PciRoot(0x0)/Pci(0x1F,0x3)</key>
	<dict>
		<key>layout-id</key>
		<data>DQAAAA==</data>
	</dict>
	<key>PciRoot(0x0)/Pci(0x1b,0x0)</key>
	<dict>
		<key>layout-id</key>
		<data>AQAAAA==</data>
	</dict>
```

### Kernel>Emulate
| key | | Comment |
| --- | --- | --- |
| Cpuid1Data | 55060A00000000000000000000000000 | Emulate Comet Lake CPU ([Reference](https://chriswayg.gitbook.io/opencore-visual-beginners-guide/advanced-topics/using-alder-lake#kernel-greater-than-emulate)) |
| Cpuid1Mask | FFFFFFFF000000000000000000000000 | Emulate Comet Lake CPU ([Reference](https://chriswayg.gitbook.io/opencore-visual-beginners-guide/advanced-topics/using-alder-lake#kernel-greater-than-emulate)) |

### Kernel>Quirks
| key | | Comment |
| --- | --- | --- |
| AppleXcpmCfgLock | true | Not needed if CFG-Lock is disabled in the BIOS settings |
| DisableIoMapper | true | Not needed if VT-D is disabled in the BIOS settings |
| PanicNoKextDump | true | |
| PowerTimeoutKernelPanic | true | |
| ProvideCurrentCpuInfo | true | For Alderlake CPU ([Reference](https://chriswayg.gitbook.io/opencore-visual-beginners-guide/advanced-topics/using-alder-lake#kernel-greater-than-quirks)) |

### Misc>Boot
| key | | Comment |
| --- | --- | --- |
| HideAuxiliary | true | Press space to show macOS recovery and other auxiliary entries |
| PickerMode | External | GUI bootpicker ([Opencore Guide](https://dortania.github.io/OpenCore-Post-Install/cosmetic/gui.html#setting-up-opencore-s-gui))|

### Misc>Debug
| key | | Comment |
| --- | --- | --- |
| AppleDebug| true | |
| ApplePanic | true | |
| DisableWatchDog | true | |
| Target | 3 | |

### Misc>Security
| key | | Comment |
| --- | --- | --- |
| AllowSetDefault | true | |
| BlacklistAppleUpdate | true | |
| ScanPolicy | 0 | |
| SecureBootModel | Default | |
| Vault | Optional | |

### NVRAM>Add>4D1FDA02-38C7-4A6A-9CC6-4BCCA8B30102
| key | Type | Value | Comment |
| --- | --- | --- | --- |
| revcpuname | String | intel® Core™ i5 12400F | CPU name |
| revcpu | Number | 1 | CPU name |

### NVRAM>Add>7C436110-AB2A-4BBB-A880-FE41995C9F82
| key | Type | Value | Comment |
| --- | --- | --- | --- |
| boot-args | String | agdpmod=pikera e1000=0 | [Opencore Guide](https://dortania.github.io/OpenCore-Install-Guide/config.plist/comet-lake.html#nvram) |
| prev-lang:kbd | Data | 656E2D55533A30 | |

### PlatformInfo>Generic
| key | Type | Value | Comment |
| --- | --- | --- | --- |
| MLB | String | - | Board Serial |
| ROM | Data | - | MAC address |
| SystemProductName | String | MacPro7,1 | SMBIOS Type |
| SystemSerialNumber | String | - | Serial |
| SystemUUID | String | - | SmUUID |

## Mother Board Info
### USB map
![usbmap_bp](images/usbmap_bp.png)
![usbmap_mb](images/usbmap_mb.png)

### USB Port Used
|      | Connector | Device                     | |
| ---- | --------- | -------------------------- | - |
| HS01 | TypeC     | Back panel UBS 3.2 Gen 2x2 | Enabled |
| HS02 | Internal  | AURA LED Controller | Enabled |
| HS03 | Internal  | Internal USB Hub ASM107x   | Enabled |
| HS04 | TypeC with Switch | MB Connector USB 3.2 Gen 2 | |
| HS05 | USB2 | Back panel USB 2.0 | Enabled |
| HS06 | USB2 | Back panel USB 2.0 | Enabled |
| HS07 | | Unknown | |
| HS08 | | Unknown | |
| HS09 | USB3 | MB Connector USB 3.2 | Enabled |
| HS10 | USB3 | MB Connector USB 3.2 | Enabled |
| HS11 | USB2 | Back panel USB 2.0 | Enabled |
| HS12 | | Unknown | |
| HS13 | | Unknown | |
| HS14 | Internal | BCM94360NG | Enabled |
| SS01 | TypeC | Back panel UBS 3.2 Gen 2x2 | Enabled | 
| SS02 | Internal | Internal USB Hub ASM107x | Enabled |
| HS03 |  TypeC with Switch | MB Connector USB 3.2 Gen 2 | |
| HS04 | | Unknown | |
| HS05 | | Unknown | |
| HS06 | | Unknown | |
| HS07 | | Unknown | |
| SS08 | USB3 | MB Connector USB 3.2 | Enabled |
| SS09 | USB3 | MB Connector USB 3.2 | Enabled |
- USB 3.2 Gen 1 Type-C and USB 3.2 Gen 1 (E1, E2 and E3) on the back panel are connected to internal USB hub ASM107x.
- Disabled USB 3.2 Gen 2 front panel connector and USB 2.0 header on motherboard.

### Audio Layout
- Built-in Audio is Realtek ALCS1220A. Layout id is 13.

## BIOS Settings
- **Advanced > CPU Configuration > Intel (VMX) Virtualization Technology**:  Enabled (default)
- **Advanced > CPU Configuration > Active Performance Cores**: All (i5 12400f has no E-Cores)
- **Advanced > CPU Configuration > Hyper-Threading**: Enabled (default)
- **Advanced > System Agent (SA) Configuration > VT-d**: Enabled (default)
- **Advanced > System Agent (SA) Configuration > Control Iommu Pre-boot Behavior**: Disable IOMMU
- **Advanced > PCI Subsystem Settings > Above 4G Decoding**: Enabled (default)
- **Advanced > PCI Subsystem Settings > Re-Size BAR Support**: Enabled (default)
- **Advanced > USB Configuration > Legacy USB Support**: Enabled
- **Advanced > USB Configuration > XHCI Hand-off**: Enabled
- **Advanced > Network Stack Configuration > Network Stack**: Disabled
- **Advanced > USB Configuration > Legacy USB Support** Enabled
- **Boot > CSM (Compatibility Support Module) > Launch CSM**: Disabled
- **Boot > Secure Boot > OS Type**: Other OS
- **Boot > Secure Boot Mode**: Custom
- **Boot > Boot Configuration > Fast Boot**: Disabled

## To Use this EFI
- Serial, Board Serial, SmUUID, and Apple ROM are deleted. Generate them using GenSMBIOS.
[SMBIOS](https://github.com/corpnewt/GenSMBIOS)  
[OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/config.plist/#creating-your-config-plist)

## Thanks / Credits
- **[Opencore Team](https://dortania.github.io/getting-started/)**
