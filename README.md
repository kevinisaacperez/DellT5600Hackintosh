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
|PCIe Card 2      |Yuobo BCM94360CD 802.11a/g/n/ac BT4.0 Network Adapter    |
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

1. CPU Cooler Fans
- With the CPU Cooler Fans, the fix was relatively easy, I bought a pair of [5-pin to 4-pin fan connector adapters](https://www.ebay.com/itm/193291905125) from ebay. Pretty much plug-and-play, you may need to use the Dell Diagnostic Tool at boot in order to get the system to test itself and remove the "CPU Fan Error" that can appear at startup.

2. System Fans
- With the system/case fans, though they are 4-pin headers, they're slimmer than the one's on the Artic F9 fans. What I did was carefully remove the headers from the motherboard, so that the pins are exposed. Then, after looking for the pinout for the Dell 4-pin header, I realized I had to swap around two cables on the connector. I then plugged them into the bare pins and powered on the system withou issues.

Other hardware related issues I had:
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
1. General
  - Boot Sequence: Legacy
2. System Configuration
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
3. Video
  - Primary Video Slot: SLOT4
4. Security
  - Intel TXT(LT-SX) Configuration: Disabled
  - TPM Security: Disabled
  - Computracer(R): Deactivated
  - Chassis Intrussion: Disbaled
  - CPU XD Support: Enabled
  - OROM Keyboard Access: Disabled
  - Admin Setup Lockout: Disabled
5. Performance
  - Multi Core Support: All
  - Intel SpeedStep: Enabled
  - C-States Control: Enabled
  - Intel TurboBoost: Enabled
  - HyperThread Control: Enabled
  - Non-Uniform Memory Access: Enabled
  - Cache Prefetch: Enable all
  - Dell Reliable Memory Technology (RMT): Disabled
6. Power Management
  - AC Recovery: Power off
  - Auto On Time: Disabled
  - Deep Sleep Control: Disabled
  - Fan Speed Control: Auto
  - Wake on LAN: Disabled
7. POST Behavior
  - Numlock LED: Enabled
  - Keyboard Errors: Enabled
  - POST Hotkeys: Enabled
8. Virtualization Support
  - Virtualization: Enabled
  - VT for Direct I/O: Enabled
9. Maintenance
  - SERR Messages: Enabled


# The not so easy part II: Getting it to boot OpenCore 0.7.1

# The very hard part: Getting to the macOS Catalina installer

# The easy part: Final touches

