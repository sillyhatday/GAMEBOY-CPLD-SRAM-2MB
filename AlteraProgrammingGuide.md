# Programming Guide

For the USB Blaster I would suggest avoiding the small ones shown below. I found them to be flakey. Talk to some CPLD and not others, even ones it could talk to it couldn't program. The same chips worked on other programmers.

![USB-Blaster-ALTERA-CPLDFPGA-Programmer-1-52150820](https://github.com/sillyhatday/GAMEBOY-MBC5-CPLD-2MB/assets/65309612/c1cf440a-68b1-4ccf-bb4e-ad689d0300b6)

I suggest this one, it has been much more solid.

![AliJTAG](https://github.com/sillyhatday/GAMEBOY-MBC5-CPLD-2MB/assets/65309612/7360df56-3824-4cea-9fbe-50ed7c374452)

This is a rough guide and you are expected to know some things already, such as how to use a Windows PC, solder SMD parts, etc.

First off, order the cart PCB from your manufacturer of choice. I use JLC for small quantity orders like this as they work out cheapest. Make sure to choose ENIG finish and 0.8mm thickness. You do not need to order the JTAG adaptor if you wish to manually solder to the test points on each cart. The adaptor does not need to be ENIG finsih.

Order your parts list for however many carts you ordered. Get them all from Aliexpress, unless you want to order legit FRAM from Digikey. The flash chip and CPLD are obsolete long ago and only available second hand.

Get your USB Blaster drivers installed. I used a github repo for the drivers. Install them through device manager. The links below have the details and drivers.

Driver Installation: https://www.terasic.com.tw/wiki/Intel_USB_Blaster_Driver_Installation_Instructions

Driver Download: https://github.com/sudhamshu091/USB-Blaster-Driver-for-DE10-lite

Assemble the cart. Make sure you have good solder joints and that any flux is thoroughly cleaned from IC pins. The JTAG interface seems sensitive and excess flux has caused me issues trying to program.

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

Now move over to your GBxCart software and test it out. Unplug the JTAG hardware and plug your cart into your GBxCart.

Once this is done, you are done with the software and programming adaptor. If you are building multiple carts then I'd suggested repeating these steps before moving any further with all of them. Any trouble programming is usually due to a bad JTAG connection, if that isn't the case then the CPLD is likely bad. I've seen totally dead ones, ones that ID and don't program and ones that program yet don't function. I've tried everything to make sure these were faulty.

Next up get the flash chip attached to the PCB along with it's supporting capacitor. Once done the cart is ready for the first of a few tests.

Move over to your GBxCart software and test it out. You'll need to manualy set the cartridge type for the first time programming. It should program whatever ROM you choose. I suggest a ROM that is the full size of the flash chip to make sure all functions on the cart
are functioning properly. Problems that you could come across are that the CPLD is dead and the flash doesn't program properly. One tell is that the process fails at 16kB due to the higher address lines not being controlled by the dead CPLD.

If things went well and programmed correctly then try it in a Gameboy to see if it boots and everything looks good. If this is the case, move onto the next step of the SRAM.

It is best to start with the SRAM supervisor IC as it is the last tricky IC to solder. Any shorts across pins on this can cause havoc with SRAM issues and even hold the Gameboy in reset (Ask me how I know). After that put everything else on the PCB except for the battery holder. It's best to test the cart in a Gameboy now and see that it still boots. If the screen stays black, don't worry, it's just the CPU being held in reset. Go back over the supervisor IC and make sure there are no shorts. They are nearly invisible on this chip.

If it boots up correctly then test the SRAM. To do this I suggest to reprogram the cartridge with a game that uses the SRAM as extra system RAM. The best game for this is Pokemon Red/Blue/Green/Yellow as the Pokemon sprites are decompressed inside the cartridge RAM, then the earliest time you see the sprites are on the title screen. If you have any sprites that look wrong, you have an issue somewhere in the SRAM circuit. If the sprites are blank then it's likely somewhere on the chip select logic. If they have small glitches then it's likely an address or data line issue.

At this point you can solder on the battery holder, stuff a battery in it and do one final test. Again using Pokemon start a new game and see if you can save the game. After that power off the console for 5 to 10 seconds and see if the save is still there. If so then you are done. If not, then you have an issue with a dead battery or still issues on the supervisor IC.

Thats it done. This is quite involved vs the FRAM version. FRAM might be dodgy and require resoldering multiple times but there is much less of other things to go wrong. So pick your poison, deal with FRAM or SRAM and it's extra supporting components. That said, I've a FRAM testing cart to upload, removing all problems by being able to bin all bad chips before using them.
