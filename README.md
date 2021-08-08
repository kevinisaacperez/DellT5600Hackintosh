# WARNING
>DO NOT DOWNLOAD AND USE THESE FILES WITHOUT KNOWING WHAT YOU ARE DOING.
>
>THESE CONFIGURATIONS WORKED FOR MY MACHINE, ***BUT MAY NOT WORK WITH YOURS!!!***
>
>I'M SHARING THEM SO THAT THEY MAY HELP YOU TROUBLESHOOT OUR UNDERSTAND PROBLEMS THAT YOU MAY FACE.
>
>***ONLY USE THESE FILES AS REFERENCE!!!***

# Dell T5600 Hackintosh
I managed to get macOS Catalina running on a Dell Precision T5600. I didn't find much about hackintoshing this particular desktop, so I hope this helps someone.

# The Challenge
So, for a while I've been trying to run an Opencore Hackintosh on this machine, but as you can imagine, almost verything is a Dell proprietary component. Another thing one could imagine is that there isn't much information about hackintoshing this particular model. 

I mostly followed [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/) and a lot of problems were fixed by Googling or trial-and-error.

# The Specs
So, as of August 7th 2021, this is my current setup:

|**Component**    |**Model**                                                |
|-----------------|---------------------------------------------------------|
|Desktop          |Dell Precision T5600                                     |
|Motherboard      |Dell GN6JF (C600 Chipset)                                |
|Power Supply     |825W Dell Proprietary                                    |
|CPU              |2x Intel Xeon E5-2630 Sandy Bridge-EP                    |
|CPU Cooling      |2x Cooler Master Hyper 212                               |
|GPU              |NVIDIA Quadro K5200 8GB                                  |
|PCIe Card 1      |Startech 2-Port USB 3.1 Card PEXUSB312C2 (ASMEDIA 2142)  |
|PCIe Card 2      |[Yuobo BCM94360CD 802.11a/g/n/ac BT4.0 Network Adapter](https://www.amazon.com/gp/product/B082X8MBMD/ref=ox_sc_act_title_1?smid=A2ZLT9J6XHPMYK&psc=1)    |
|RAM              |4x4GB Hynix PC3-12800R                                   |
|SSD              |Samsung SSD PM851 2.5 7mm 256GB                          |
|HDD              |Hitachi HDS722020ALA330 2TB                              |
|Airflow          |3x Arctic F9 PWM Fans                                    |

For the motherboard, these are the controllers that interest us:

|**Device**       |**Controller**                                           |
|-----------------|---------------------------------------------------------|
|CPU              |C600 Chipset (similar to X79)                            |
|Audio            |RealTek ALC269 High Definition Audio                     |
|Network          |Intel 82759                                              |


# Hardware
After reading through Dortania's OpenCore guide and making sure that the hardware I had was supported, or at least compatible, I began building.

I already had verified that all components worked both standalone and together in Windows 10, but I had a pretty big issue that I had to fix. Anyone who has tried to mod a Dell PC has encountered the proprietary 5-pin and 4-pin fan headers, so here's what I did:

**1. CPU Cooler Fans**
- With the CPU Cooler Fans, the fix was relatively easy, I bought a pair of [5-pin to 4-pin fan connector adapters](https://www.ebay.com/itm/193291905125) from ebay. Pretty much plug-and-play, you may need to use the Dell Diagnostic Tool at boot in order to get the system to test itself and remove the "CPU Fan Error" that can appear at startup.

**2. System Fans**
- With the system/case fans, though they are 4-pin headers, they're slimmer than the one's on the Artic F9 fans. What I did was carefully remove the headers from the motherboard, so that the pins are exposed. Then, after looking for the pinout for the Dell 4-pin header, I realized I had to swap around two cables on the connector. I then plugged them into the bare pins and powered on the system withou issues.

**Other hardware related issues I had:**
- Eliminated the CD-R/DVD reader, since I didn't have use for it
- Can't put the sidepanel back on since I didn't check the clearance for the coolers (can be fixed with lower profile LGA2011 (narrow ILM) coolers)

My build has these PCIe slots populated:
|**Slot**         |**Device**                                               |
|-----------------|---------------------------------------------------------|
|1                |Startech 2-Port USB 3.1 Card PEXUSB312C2 (ASMEDIA 2142)  |
|2                |EMPTY                                                    |
|3                |Yuobo BCM94360CD 802.11a/g/n/ac BT4.0 Network Adapter    |
|4                |NVIDIA Quadro K5200 8GB                                  |
|5                |EMPTY, Covered by GPU                                    |

# BIOS Configuration
Update the BIOS to the latest release, in my case it was A19. If you can update it within Windows (before erasing the OS) it'll be easier.

These are the BIOS settings I used, some of the options areeasily identifiable in the OpenCore guide, though after searching I found [this guide from cstrouse](https://github.com/cstrouse/Dell-T3610-Hackintosh) I found it much simpler to assign some option in BIOS. Remember, that the BIOS between Dell Precision models is always slightly different:
**1. General**
  - Boot Sequence: Legacy
**2. System Configuration**
  - Integrated NIC: Enabled
  - Serial Port: Disabled
  - AHCI Operation: AHCI
  - SATA/RAID Operation: Disabled
  - Drives: Enable all
  - SMART Reporting: Disabled
  - USB Configuration: Enable all
  - Optional Boot Sequence: Disabled
  - PCI Bus Configuration: 256 PCI Buses
  - PCI MMIO Space Size: Large
  - HDD Fans: Disabled
  - Audio: Enabled
**3. Video**
  - Primary Video Slot: SLOT4
**4. Security**
  - Intel TXT(LT-SX) Configuration: Disabled
  - TPM Security: Disabled
  - Computracer(R): Deactivated
  - Chassis Intrussion: Disbaled
  - CPU XD Support: Enabled
  - OROM Keyboard Access: Disabled
  - Admin Setup Lockout: Disabled
**5. Performance**
  - Multi Core Support: All
  - Intel SpeedStep: Enabled
  - C-States Control: Enabled
  - Intel TurboBoost: Enabled
  - HyperThread Control: Enabled
  - Non-Uniform Memory Access: Enabled
  - Cache Prefetch: Enable all
  - Dell Reliable Memory Technology (RMT): Disabled
**6. Power Management**
  - AC Recovery: Power off
  - Auto On Time: Disabled
  - Deep Sleep Control: Disabled
  - Fan Speed Control: Auto
  - Wake on LAN: Disabled
**7. POST Behavior**
  - Numlock LED: Enabled
  - Keyboard Errors: Enabled
  - POST Hotkeys: Enabled
**8. Virtualization Support**
  - Virtualization: Enabled
  - VT for Direct I/O: Enabled
**9. Maintenance**
  - SERR Messages: Enabled

# Booting OpenCore 0.7.1
This part only took a couple of hours.

So, first step is creating the USB. I used a spare 16GB I had laying around and used another Hackintosh (a Dell Precision M6800) to download MacOS 10.15.7 Catalina full installer, instead of the recovery image. Everything went according to Dortania's Guide, I justa had to make sure to leave everithing on LEGACY MODE, instead of UEFI; I don't know why, but OpenCore wouldn't boot at all if I tried an UEFI Install.

After that I added the base X64 files, and continued following the guide. One note I have to make is that I used the Debug version of OpenCore 0.7.1 in case I had problems and, oh boy, did it come in handy.

**1. Drivers**
  - HfsPlusLegacy.efi (needed because legacy install)
  - OpenCanopy.efi (needed later for OpenCore GUI)
  - OpenRuntime.efi (Required)
  - OpenPartitionDxe.efi (Don't know why, wouldn't boot without it)
  - OpenUsbKbDxe.efi (needed because legacy install)

**2. Kexts**
This part gave me the largest headache, and is probably one of the reasosn why it took me 3 weeks instead of a couple of days to finish this install. 
  - VirtualSMC (Required)
    - SMCDellSensors
    - SMCProcessor
    - SMCSuperIO
  - Lilu (Required)
  - WhateverGreen (Required)
  - AppleALC (Required)
  - IntelMausi (Required for working ethernet port, added after I installed macOS)
  - USBInjectAll (Required for USB2 Type A ports) (Note: the Bluetooth/WiFi card works natively without Kexts, but requires USBInjectAll for the internal USB port to work.)
  - mXHCD (Required for USB3 Type A ports, added after I installed macOS)
  - ASMedia (Required for USB3 Type C ports added with PCIe card, added after I installed macOS)
  - XHCI-unsupported (Needed for non-native USB controllers)
  - AppleMCEReporterDisabler (Don't know why, but wouldn't boot without it)
  - CtlnaAHCIPort (Don't know if it boot without it, too scared to try)
  - SATA-unsupported (Adds support for a large variety of SATA controllers, couldn't install macOS without it)

**3. ACPI**
Reason #2 for this taking way longer than I expected. All were made using the ssdtTime method, for which you'll need Windows or Linux installed on the PC you want to convert to a Hackintosh.
  - SSDT-EC: Embedded Controller. Required on Sandy Bridge-EP processors.
  - SSDT-HPET: Fixing IRQ Conflicts. Couldn't boot without it.
  - SSDT-PM: Power Management. Requiered to connect to Apple's XCPM. Created with CPUFriend after install.
  - SSDT-UNC: Uncore Bridge. Required for C602/X79 boards.

**4. Config plist**
  - I've removed MLB, ROM, UUID and other serials from the file I've provided, so follow Dortania's Guide in order to generate valid ones. Do not use the file as-is, I urge you to read the Guide first and use my config.plist as a reference. 
  - I'm sorry to say that I've tinkered to much with the plist and don't remember exactly what I've changed compared to the one created with the help from the Guide. It was this file that consumed me for about two weeks (on and off), and was responsible of several sleepless and/or restless nights, some of which involved me dreaming about this file. 
  - A previous attempt by a [reddit user with this same model (using Clover)](https://www.reddit.com/r/hackintosh/comments/5v9x1w/dell_precision_t5600_32t32g_dp_xeon_successful/) helped me figure out some boot arguments to use, and also convinced me to change the system fans.
  - I was stuck for a long time with the [[EB|#LOG:EXITBS:START] error](https://dortania.github.io/OpenCore-Install-Guide/troubleshooting/extended/kernel-issues.html#stuck-on-eb-log-exitbs-start), and basically tried everything until it worked.
  - Also, I got stuck at the ["Waiting for Root Device" or Prohibited Sign error](https://dortania.github.io/OpenCore-Install-Guide/troubleshooting/extended/kernel-issues.html#waiting-for-root-device-or-prohibited-sign-error), I reset NVRAM and changed UEFI/Quirks/ReleaseUsbOwnership/True.

**5. Tools**
  - OpenShell (Required)
  - OpenRuntime (Used for determining KASLR slide values, removed post install)

# The macOS Catalina installer
Once past the now very hopeless verbose boot, I went directly to the Disk Utility and formated the disk I was using for this installation. Went back and hit the Install Catalina button.

One issue that did present itself was the [Your Mac needs a firmware update in order to install to this volume](https://dortania.github.io/OpenCore-Install-Guide/troubleshooting/extended/userspace-issues.html#stuck-on-your-mac-needs-a-firmware-update-in-order-to-install-to-this-volume), which was resolved following the Guide.

Let it install and be ready for restarts, because you'll need to manually choose the new option in OpenCore.

# Final touches
Well, the last thing to do now that macOS is installed is copy all the contents of the EFI Partition on your USB to the EFI Partition on your HDD. Mount the EFI partitions using [Mount-EFI by corpnewt](https://github.com/corpnewt/MountEFI). One thing you'll have to do is run the [DuetPkg on the HDD because we installed in legacy mode](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/mac-install.html#legacy-setup), just like when we made the install USB. 

After that, we'll have to generate our SSDT-PM using CPUFriend, and be sure to modify our config.plist using the OC Snapshot in ProperTree.

Restart and you should have a fully functioning Hackintosh. Follow [Dortania's Post-Install Guide](https://dortania.github.io/OpenCore-Post-Install/) to finish setting up your device.
