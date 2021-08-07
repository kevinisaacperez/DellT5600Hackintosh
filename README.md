# WARNING

DO NOT DOWNLOAD AND USE THESE FILES WITHOUT KNOWING WHAT YOU ARE DOING.
THESE CONFIGURATIONS WORKED FOR MY MACHINE, **BUT MAY NOT WORK WITH YOURS!!!**
I'M SHARING THEM SO THAT THEY MAY HELP YOU TROUBLESHOOT OUR UNDERSTAND PROBLEMS THAT YOU MAY FACE.
**ONLY USE THESE FILES AS REFERENCE!!!**

# Dell T5600 Hackintosh
I managed to get macOS Catalina running on a Dell Precision T5600. I didn't find much about hackintoshing this particular desktop, so I hope this helps someone.

# The Challenge
So, for a while I've been trying to run an Opencore Hackintosh on this machine, but as you can imagine, almost verything is a Dell proprietary component. Another thing one could imagine is that there isn't much information about hackintoshing this particular model. 

I mostly followed [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/) and a lot of problems were fixed by Googling or trial-and-error.

# The Specs
So, as of August 7th 2021, this is my current setup:

|Component        |Model                                                    |
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

|Device           |Controller                                               |
|-----------------|---------------------------------------------------------|
|CPU              |C600 Chipset (similar to X79)                            |
|Audio            |RealTek ALC269 High Definition Audio                     |
|Network          |Intel 82759                                              |


# The not so easy part I: Hardware requirements and BIOS configuration

# The not so easy part II: Getting it to boot OpenCore 0.7.1

# The very hard part: Getting to the macOS Catalina installer

# The easy part: Final touches

