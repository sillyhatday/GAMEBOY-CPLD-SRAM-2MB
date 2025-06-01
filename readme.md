# GAMEBOY-CPLD-SRAM-2MB

## Introduction:

This is my take of a CPLD based flash cart. This is my second version of it, and my final version. I may update to a 4MB version though it is not a priority on my project list.

The whole project is based from Alex's project from years back. Without that project, I would never have picked this up and spend my time figuring out how to work with CPLDs. I am not a programmer and do not claim to be. Picking up Alex's code I can see how things work with my previous knowledge of how carts operate.
This cart version uses SRAM like the original project from Alex.

As much as I love using FRAM to bring this old technology towards the modern age in terms of convenience, it is technically not compatible due to timing reasons, it's really expensive new, China dodgy parts don't always work and on the whole it's a pain.
Embrace SRAM, it's cheap and available. The battery life is not an issue anyway as my original Pokemon Red, Blue and Yellow carts are still using the original batteries with lots of life left in them yet after 25 years.

As well as a thanks to Alex for sharing his project for us to use, I want to thank Bucket Mouse for also sharing his work with everyone. The SRAM supervisor circuit is directly lifted from his cartridge projects and saved me a lot of time and frustration designing my own.

### Alex's project can be found here: https://github.com/insidegadgets/Gameboy-MBC5-MBC1-Hybrid
### Buckets projects can be found here: https://github.com/MouseBiteLabs

## Advantages vs disadvantages:

### Advantages:

	+ No need for sourcing a donor cartridge
	+ Multi MBC compatibility
	+ Cheaper to build vs a donor cart (initially not, more made, more saved)
 	+ No messing with expensive FRAM or dodgy back ally FRAM

### Disadvantages:

	- Parts are all obsolete and found second hand
	- Programming the CPLD is non-trivial
	- Extra components required to program CPLD
 	- More circuit board components required
  	- Requires a battery to save data

## Prerequisites

You are going to need a bunch of stuff to complete this project.

	* The full components parts list and cart PCB
	* (Optional) JTAG adaptor PCB
	* Altera USB Blaster for JTAG (Cheap copies work, see below)
	* Windows PC with Quartus II Programmer or Quartus II Web Edition
	* GBxCart and related software
 	* Modified Gameboy cart shell or (optional) 3D printed Game Gear cart adaptor
	* Equipment to solder tiny SMD components
 	* Skills to solder tiny SMD components

## Parts List

### Cartridge x1

| Part No. | Package | Qty |
| -------- | ------- | --- |
| 29F016 | TSOP48 | 1 |
| IS62C256 | SOP28 | 1 |
| EPM3064A/32A | TQFP44 | 1 |
| AP2127K-3.3TRG1 | SOT23-5 | 1 |
| TPS3613-01DGSR | 10VSSOP | 1 |
| 2N7002 | SOT23 | 1 |
| Cap 100nF | 0603 | 5 |
| Cap 1uF | 0603 | 1 |
| Res 10K | 0603 | 1 |
| Res 49K9 | 0603 | 1 |
| Res 130K | 0603 | 1 |
| Bat Holder | AliExp | 1 |

[ IS62C256 can be replaced by nearly any SOP28 SRAM chip with a capacity of 256kbit. The ISSI part is dumb cheap brand new from reputable sellers.
Other alternatives include but not limited to: AS6C6256, CY62256, R1LP5256 ]

### JTAG Adaptor

| Part No. | Qty |
| -------- | --- |
| DS Lite Cart Conn | 1 |
| USB C USB4125 | 1 |
| Pin Headers | 2x5-P2.54mm |
| GG Adaptor or Shell | 1 |
| USB Blaster | 1 |

## Compatibility

Alex who wrote this code originally, designed it to be an MBC5 emulation. It behaves well enough as a MBC5, that Gameboys and Flash programmers do not seem to care. One difference from the top of my head, after reading Alex's blog, is that this CPLD version does not map bank 0 as true hardware does. This should never be an issue, as bank 0 is always available in the lower 16KB address space [0x0000-3FFF]. On real hardware, if you mapped bank 0 to the upper 16KB [0x4000-7FFF], you would get the data always available in 0x0000-3FFF in 0x4000-7FFF. In this implementation, it will do as the MBC1 does, by interpreting a bank 0 map request as a bank 1 request.

In theory this should never cause an issue. Logically there is no reason to map bank 0 in this use case. A much better idea would be, to map bank 1 as the default in the upper address space [0x4000-7FFF]. This way you would have the first 32KB chunk of the ROM available like any 32KB size game. The only way this will cause and issue is if the game programmer coded a zero bank map and then tried reading the upper memory space [0x4000-7FFF].

So why did Nintendo remove this idiot proof feature from their first mapper in the later ones? In my opinion, I think they realised mapping 0 bank is a waste of time and it doesn't hurt anything to do. But mostly as MBC1 and MBC5 use a second register to select the highest address bit in their extended ROM mode. Doing this you end up with holes in the address space you need to keep track of. For MBC1 you have 512KB of ROM space available, in extended mode you have 2MB available. The quadrupling of space uses another register to store 2 extra bits. These are then output on two extra I/O pins. If you want the first bank of the 1 to 1.5MB ROM space, that is bank zero, which switches to a 1. Each 512KB ROM space is missing bank 0. In MBC5 this does not exist as it is able to map bank zero, therefore, you do not have to keep track of things.

The theory is that this should be 100% compatible with MBC5. That said, it is not programmed to behave exactly like an MBC5, so there could be a game out there that doesn't work.

## PCB Versions

### V1.0

First version, confirmed working.

![Gameboy CPLD Cart + SRAM Front](https://github.com/user-attachments/assets/bfedf3a0-f963-46ce-9725-eeb6cd695a92)

![Gameboy CPLD Cart + SRAM Back](https://github.com/user-attachments/assets/0385e6f7-6376-4245-91c1-fe59061d6bce)

## Firmware Versions

### V1.0

Original code from Alex's blog

### V1.1

Removed internal FRAM chip select logic to lower cartridge power consumption 1 to 3%

### V2.0

Changed project compiler settings to lower cartridge power consumption 36 to 41%

## Links

Here are the links mentioned through the readme. They are already in the readme, but also here for aid of finding them.

Driver Installation: https://www.terasic.com.tw/wiki/Intel_USB_Blaster_Driver_Installation_Instructions

Driver Download: https://github.com/sudhamshu091/USB-Blaster-Driver-for-DE10-lite

Game gear Adaptor: https://www.thingiverse.com/thing:5830799

Alternative Download for Quartus: https://archive.org/details/quartus-iiprogrammer-and-signal-tap-ii-13.1.0.162.7z

## Extra Links

Once again, thankyou to everyone for sharing their work with everyone for free

https://shop.insidegadgets.com

https://github.com/insidegadgets

https://www.instagram.com/inside.gadgets

https://github.com/bytendomods

https://github.com/Bucket-Mouse

## License

This work is licensed under a Creative Commons Attribution-ShareAlike 4.0 International License. You are able to copy and redistribute the material in any medium or format, as well as remix, transform, or build upon the material for any purpose (even commercial) - but you must give appropriate credit, provide a link to the license, and indicate if any changes were made.
