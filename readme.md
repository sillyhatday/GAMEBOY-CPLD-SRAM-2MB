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

### Disadvantages:

	- Parts are all obsolete and found second hand
	- Programming the CPLD is non-trivial
	- Power use is high
	- Extra components required to program CPLD
 	- More circuit board components required

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

For the USB Blaster I would suggest avoiding the small ones shown below. I found them to be flakey. Talk to some CPLD and not others, even ones it could talk to it couldn't program. The same chips worked on other programmers.

![USB-Blaster-ALTERA-CPLDFPGA-Programmer-1-52150820](https://github.com/sillyhatday/GAMEBOY-MBC5-CPLD-2MB/assets/65309612/c1cf440a-68b1-4ccf-bb4e-ad689d0300b6)

I suggest this one, it has been much more solid.

![AliJTAG](https://github.com/sillyhatday/GAMEBOY-MBC5-CPLD-2MB/assets/65309612/7360df56-3824-4cea-9fbe-50ed7c374452)

# Guide

This is a rough guide and you are expected to know some things already, such as how to use a Windows PC, solder SMD parts, etc.

First off, order the cart PCB from your manufacturer of choice. I use JLC for small quantity orders like this as they work out cheapest. Make sure to choose ENIG finish and 0.8mm thickness. You do not need to order the JTAG adaptor if you wish to manually solder to the test points on each cart. The adaptor does not need to be ENIG finsih.

Order your parts list for however many carts you ordered. Get some from Aliexpress and some from a reputable electronics seller. The flash chip and CPLD are obsolete long ago and only available second hand.

Make sure you have good solder joints and that any flux is thoroughly cleaned from IC pins. The JTAG interface seems sensitive and excess flux has caused me issues trying to program.

For this, I think it's worth adding a process to the build. The FRAM version you can just smash all the parts on at once. With this there are so many points that can go wrong and cause issues that don't seem logical at the time.
Firstly, start by adding the CPLD and the voltage regulator, then place the supporting components for them. Those being the capacitors around the CPLD and voltage regulator, do not skip this step as the capacitors are required for stability, it will cause problems otherwise.
That is all is needed to be able to program the CPLD. I'd suggest getting this step out of the way now, so as to ensure that a mistake later does not interfere with the JTAG process. 
Make sure you have good solder joints and that any flux is thoroughly cleaned from IC pins. The JTAG interface seems sensitive and excess flux has caused me issues trying to program.

Get your USB Blaster drivers installed. I used a github repo for the drivers. Install them through device manager. The links below have the details and drivers.

Driver Installation: https://www.terasic.com.tw/wiki/Intel_USB_Blaster_Driver_Installation_Instructions

Driver Download: https://github.com/sudhamshu091/USB-Blaster-Driver-for-DE10-lite

Make sure to download the programmer software from here or source it yourself if you wish. The install is simple, just click through and wait. I have also uploaded it here:

https://archive.org/details/quartus-iiprogrammer-and-signal-tap-ii-13.1.0.162.7z

Insert your assembled cart into the JTAG adaptor using the 3D printed shim so that the cart can be inserted upside down. (The side with the 6 cart edge pins). If you don’t have  access to a shim, you can sacrifice an old Gameboy cart by cutting it up so that your cart can fit in it upside down.

Launch the Quartus II Programmer software you just installed.

![2 MainProgUSBBlasterSelected](https://github.com/sillyhatday/GAMEBOY-MBC5-CPLD-2MB/assets/65309612/70f33a9c-51d2-422a-b8f0-de7029f098a5)

Make sure that the USB Blaster is detected and selected in Quartus. If not click “Hardware Setup…” and find it.

![1USB Blaster selection](https://github.com/sillyhatday/GAMEBOY-MBC5-CPLD-2MB/assets/65309612/b13a9b11-7825-42f8-b04a-1348c170efce)

Once selected select the POF file for the CPLD you are using, 3064A or 3032A.

![3 FileSelected](https://github.com/sillyhatday/GAMEBOY-MBC5-CPLD-2MB/assets/65309612/509d84be-c925-4928-9327-ad6c75a6f1af)

If the CPLD is unproven to work yet, it may also be programmed from its previous life. Click “Erase” and press start. If your JTAG is connected properly and working, it should erase quickly. If problems continue, recheck JTAG connections, solder joints and flux residue. By this point, if things still do not work, then the CPLD is already programmed with JTAG disabled (requiring an external programmer) or it is damaged internally.

![4 EraseCPLD](https://github.com/sillyhatday/GAMEBOY-MBC5-CPLD-2MB/assets/65309612/0c7be1bc-0fed-4b33-837c-dfc981d2b265)

Once erased, it is worth doing a “Blank-Check” to be sure that the erase worked correctly. If this fails, then it is not blank.

![5 BlankCheckCPLD](https://github.com/sillyhatday/GAMEBOY-MBC5-CPLD-2MB/assets/65309612/dcc28a9e-5c8f-4fdf-b71e-3b5c782faf33)

Finally time to program the CPLD. Check “Program/Configure” and “Verify”, then click start for the last time. It should take about 2 seconds. If you have any trouble, check your JTAG connections, reflow the CPLD, make sure the adaptor board is powered, clean off flux residue.

![6 ProgVerifyCPLD](https://github.com/sillyhatday/GAMEBOY-MBC5-CPLD-2MB/assets/65309612/22686784-cfa1-4366-9776-a0bd3121f770)

Once this is done, you are done with the software and programming adaptor. If you are building multiple carts then I'd suggested repeating these steps before moving any further with all of them. Any trouble programming is usually due to a bad JTAG connection, if that isn't the case then the CPLD is likely bad. I've seen totally dead ones, ones that ID and don't program and ones that program yet don't function. I've tried everything to make sure these were faulty.

Next up get the flash chip attached to the PCB along with it's supporting capacitor. Once done the cart is ready for the first of a few tests.

Move over to your GBxCart software and test it out. You'll need to manualy set the cartridge type for the first time programming. It should program whatever ROM you choose. I suggest a ROM that is the full size of the flash chip to make sure all functions on the cart
are functioning properly. Problems that you could come across are that the CPLD is dead and the flash doesn't program properly. One tell is that the process fails at 16kB due to the higher address lines not being controlled by the dead CPLD.

If things went well and programmed correctly then try it in a Gameboy to see if it boots and everything looks good. If this is the case, move onto the next step of the SRAM.

It is best to start with the SRAM supervisor IC as it is the last tricky IC to solder. Any shorts across pins on this can cause havoc with SRAM issues and even hold the Gameboy in reset (Ask me how I know). After that put everything else on the PCB except for the battery holder. It's best to test the cart in a Gameboy now and see that it still boots. If the screen stays black, don't worry, it's just the CPU being held in reset. Go back over the supervisor IC and make sure there are no shorts. They are nearly invisible on this chip.

If it boots up correctly then test the SRAM. To do this I suggest to reprogram the cartridge with a game that uses the SRAM as extra system RAM. The best game for this is Pokemon Red/Blue/Green/Yellow as the Pokemon sprites are decompressed inside the cartridge RAM, then the earliest time you see the sprites are on the title screen. If you have any sprites that look wrong, you have an issue somewhere in the SRAM circuit. If the sprites are blank then it's likely somewhere on the chip select logic. If they have small glitches then it's likely an address or data line issue.

At this point you can solder on the battery holder, stuff a battery in it and do one final test. Again using Pokemon start a new game and see if you can save the game. After that power off the console for 5 to 10 seconds and see if the save is still there. If so then you are done. If not, then you have an issue with a dead battery or still issues on the supervisor IC.

Thats it done. This is quite involved vs the FRAM version. FRAM might be dodgy and require resoldering multiple times but there is much less of other things to go wrong. So pick your poison, deal with FRAM or SRAM and it's extra supporting components. That said, I've a FRAM testing cart to upload, removing all problems by being able to bin all bad chips before using them.

## Compatibility

Alex who wrote this code originally, designed it to be an MBC5 emulation. It behaves well enough as a MBC5, that Gameboys and Flash programmers do not seem to care. One difference from the top of my head, after reading Alex's blog, is that this CPLD version does not map bank 0 as true hardware does. This should never be an issue, as bank 0 is always available in the lower 16KB address space [0x0000-3FFF]. On real hardware, if you mapped bank 0 to the upper 16KB [0x4000-7FFF], you would get the data always available in 0x0000-3FFF in 0x4000-7FFF. In this implementation, it will do as the MBC1 does, by interpreting a bank 0 map request as a bank 1 request.

In theory this should never cause an issue. Logically there is no reason to map bank 0 in this use case. A much better idea would be, to map bank 1 as the default in the upper address space [0x4000-7FFF]. This way you would have the first 32KB chunk of the ROM available like any 32KB size game. The only way this will cause and issue is if the game programmer coded a zero bank map and then tried reading the upper memory space [0x4000-7FFF].

So why did Nintendo remove this idiot proof feature from their first mapper in the later ones? In my opinion, I think they realised mapping 0 bank is a waste of time and it doesn't hurt anything to do. But mostly as MBC1 and MBC5 use a second register to select the highest address bit in their extended ROM mode. Doing this you end up with holes in the address space you need to keep track of. For MBC1 you have 512KB of ROM space available, in extended mode you have 2MB available. The quadrupling of space uses another register to store 2 extra bits. These are then output on two extra I/O pins. If you want the first bank of the 1 to 1.5MB ROM space, that is bank zero, which switches to a 1. Each 512KB ROM space is missing bank 0. In MBC5 this does not exist as it is able to map bank zero, therefore, you do not have to keep track of things.

The theory is that this should be 100% compatible with MBC5. That said, it is not programmed to behave exactly like an MBC5, so there could be a game out there that doesn't work.

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

    
