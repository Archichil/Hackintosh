# Lenovo Yoga slim 7 Pro (14ACH5) Hackintosh

## Notes

**This EFI works on Sonoma 14.7.4**

**SMBIOS info is deleted, you need to generate it by your own and fill in config.plist (use [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS))**

**OpenCore 1.0.4 Debug version**

---
### Screenshot
![overview](https://github.com/user-attachments/assets/db983360-b21f-49cf-91ed-e6c544125c18)

---
### What's not working
1. Sleep
2. Network card (Wi-Fi, Bluetooth)

### Laptop configuration

---
| Components                      | Model                | Compatibility                                                                              |
|:--------------------------------|:---------------------|:-------------------------------------------------------------------------------------------|
| CPU                             | AMD Ryzen 7 5800H    | ✅                                                                                          |
| iGPU                            | AMD Radeon Vega 8    | ✅                                                                                          |
| Network card                    | MTK7921              | ❌️                                                                                         |
| SSD                             | Samsung T7 Portable  | ✅，original SKHynix PC711 NVMe (SKHynix proprietary controller is not supported by MacOS) ❌ |
| Trackpad                        | I2C HID              | ✅                                                                                          |
| Keyboard                        | PS2 controller       | ✅                                                                                          |
| Bluetooth                       | MTK7921              | ❌                                                                                          |
| Audio / 3.5mm jack / Microphone | ALC287               | ✅                                                                                          |
| RAM                             | SKHynix DDR4 3200MHz | ✅                                                                                          |
| USB                             | USBMapping           | ✅                                                                                          |

---
## EFI Specification

---
### ACPI
| SSDT           | Purpose                                                |
|:---------------|:-------------------------------------------------------|
| SSDT-PLUG-ALT  | Fixes CPU definitions                                  |
| SSDT-EC        | Adds a fake Embedded Controller device                 |
| SSDT-HPET      | Fixes IRQ conflicts                                    |
| SSDT-SBUS-MCHC | Fixes AppleSMBus                                       |
| SSDT-USBX      | Enables USB Power Management                           |
| SSDT-XOSI      | Spoof macOS to Windows for some ACPI features.         |
| SSDT-ALS0      | NootedRed provided, for brightness control             |
| SSDT-PNLF      | NootedRed provided, for brightness control             |
| SSDT-rmne      | Fixing i-services by injecting fake Ethernet(en0) kext |


---
### Kexts
| Kext                                                                                                                  | Purpose                                                                                                                                                                               |
|:----------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [AMDRyzenCPUPowerManagement](https://github.com/trulyspinach/SMCAMDProcessor)                                         | AMD CPU power management                                                                                                                                                              |
| [AppleALC](https://github.com/acidanthera/AppleALC)                                                                   | Audio fix                                                                                                                                                                             |
| [AppleMCEReporterDisabler](https://github.com/acidanthera/bugtracker/files/3703498/AppleMCEReporterDisabler.kext.zip) | Disables AppleIntelMCEReporter which causes panics on AMD CPUs                                                                                                                        |
| [BrightnessKeys](https://github.com/acidanthera/BrightnessKeys)                                                       | Automatic handling of brightness keys based on ACPI Specification. Requires Lilu 1.2.0 or newer.                                                                                      |
| [ECEnabler](https://github.com/1Revenger1/ECEnabler)                                                                  | Allows reading Embedded Controller fields over 1 byte long, vastly reducing the amount of ACPI modification needed (if any) for working battery status.                               |
| [Lilu](https://github.com/acidanthera/Lilu)                                                                           | Patch Engine                                                                                                                                                                          |
| [NootedRed](https://github.com/ChefKissInc/NootedRed)                                                                 | The AMD Vega iGPU support kext                                                                                                                                                        |
| [NullEthernet](https://github.com/RehabMan/OS-X-Null-Ethernet)                                                        | A "Null" (Fake) Ethernet driver for OS X meant to be used when you have no working Ethernet or AirPort (i-services fix)                                                               |
| [RestrictEvents](https://github.com/acidanthera/RestrictEvents)                                                       | Lilu Kernel extension for blocking unwanted processes causing compatibility issues on different hardware and unlocking the support for certain features restricted to other hardware. |
| [SMCAMDProcessor](https://github.com/trulyspinach/SMCAMDProcessor)                                                    | Power management, monitoring and VirtualSMC plugin for AMD processors                                                                                                                 |
| [SMCBatteryManager](https://github.com/acidanthera/VirtualSMC)                                                        | Battery reading                                                                                                                                                                       |
| [USBToolBox](https://github.com/USBToolBox/tool)                                                                      | USB mapping tool supporting Windows and macOS. It allows for building a custom injector kext from Windows and macOS                                                                   |
| [UTBMap](https://github.com/USBToolBox/tool)                                                                          | Kext used for attaching USBToolBox to all PCIe USB controllers.                                                                                                                       |
| [VirtualSMC](https://github.com/acidanthera/VirtualSMC)                                                               | Advanced Apple SMC emulator in the kernel. Requires Lilu for full functioning.                                                                                                        |
| [VoodooI2C](https://github.com/VoodooI2C/VoodooI2C)                                                                   | Intel I2C controller and slave device drivers for macOS                                                                                                                               |
| [VoodooI2CHID](https://github.com/VoodooI2C/VoodooI2C)                                                                | Intel I2C controller and slave device drivers for macOS                                                                                                                               |
| [VoodooPS2Controller](https://github.com/acidanthera/VoodooPS2)                                                       | PS/2 keyboard，need to disable voodoops2 input as it will conflict with i2c's input in config.plist                                                                                    |
