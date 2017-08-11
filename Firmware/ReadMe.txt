Where is CC programmer's firmware?
--------------------------------------

Sadly, because of TI's developer license, I'm not able to share their original firmware for CC programer.
Luckily, it's easy do download it from their webiste. 

1.) You can either download XDS emulation software package from here: http://processors.wiki.ti.com/index.php/XDS_Emulation_Software_Package
	Or you can download entire Code Composer Studio V7 (CCS V7) from here: http://processors.wiki.ti.com/index.php/Download_CCS#Code_Composer_Studio_Version_7_Downloads
	If your intention is to install CCS V7, be sure to check "Install XDS Emulation" during installation process.

2.) If you downloaded CCS V7: 
	In CMD, navigate to <CCS V7 install dir>\ccsv7\ccs_base\common\uscif\xds110
    If you downloaded XDS emulation software package: 
	In CMD, navigate to <XDS emu install dir>\ccsv7\ccs_base\common\uscif\xds110

3.) Connect your CC Programmer to your computer via USB. Since TI packs their CC programmer MCU with the newest firmware at the day of production, you will be able to program it via USB. Make sure that you connected only one CC programmer. 

4.) In CMD, run xdsdfu -m
    That will put device into DFU mode, which allows us to program it. Wait until your computer recognises debug probe as in DFU. 

5.) In CMD, run xdsdfu -f firmware.bin -r
    That will program the firmware. After it's done, CC programmer is ready to use. Disconnect it from your computer and connect it again. 

Changing serial number of CC programmer:
------------------------------------------

Maybe you'll have to do this, just to make sure everything works in order. Or if you have more than one CC programmer, this will allow you to connect more than one CC programmer to your computer. 

1.) Plug CC programmer into your computer. Make sure that you connected only one CC programmer to your computer. 

2.) Navigate to xdsdfu in CMD, as described in proramming steps. 

3.) In CMD, run xdsdfu -m
    Wait until your computer recognises debug probe in DFU. 

4.) run xdsdfu -n XXXX -r
    Where is XXXX serial number of your CC programmer. It can be any combination of letters and numbers, from 1 to 4 characters in length. 

5.) Once process is completed, your CC programmer is ready for use. Disconnect it and connect it again to your computer. 

If you have older CC programmer chip, or if your CC programmer is bricked:
---------------------------------------

1.) Disconnect your CC programmer from computer.

2.) Connect TP6 to ground. While TP6 is connected to ground, connect your CC programmer to computer. 

3.) Your CC programmer should now be in DFU mode. In CMD, navigate to xdsdfu and run xdsdfu -f firmware.bin -r

3.1.) If your device is not in DFU mode, unplug it, disconnect TP6 from Ground pin and connect TP3 instead. Then connect CC programmer again, and repeat step 3. 

If you have even older chip (should not happen, but you never know):
---------------------------------------------------------------------
You'll have to use JTAG. You can use another CC programmer, or any other (even one based on FT2232H). 
JTAG pins are described as it follows: 
TP1 - 3V3
TP2 - RST
TP3 - TCK
TP4 - TMS
TP5 - TDI
TP6 - TDO
TP7 - GND

1.) Obtain a copy of LM Flash Programmer from here: http://www.ti.com/tool/lmflashprogrammer and install it.
2.) Connect JTAG probe of your choice to test points as described. DO NOT connect your CC programmer to computer. You may connect CC programmer to external power source, but then leave TP1 unconnected. 
3.) Open LM FLash Programmer. In Interface section, choose ICDI (eval board) and as port choose JTAG. Keep speed at 1000000 Hz. For Clock source, select "Use the selected crystal Value" and set crystal value at 16 MHz. 
4.) In Program tab, press "browse" button and navigate to boot_loader.bin. It's placed in same directory as xdsdfu tool. Select "erase entire flash". You can select "verify after program" if you want to. As a program address offset, use 0x0000.
5.) Press "program" button. 
6.) In Program tab, again press "browse" button and navigate to "firmware.bin". It's in same directory as xdsdfu tool. Now select "erase necessary pages". As a program address offset, use 0x4000. 
7.) Press "program" button. 
8.) Once flashing is done, disconnect your CC programmer from power source and JTAG probe, and connect your CC programmer to your computer. Your CC programmer is now ready for use. 

-----------------------------------------

What if I made my own CC programmer, and I don't have JTAG probe?
--------------------------------------
I haven't tested it yet, but you'll probably have to use LM Flash programmer to program the write bootloader. After that, you can use xdsdfu. 
Otherwise, if your computer has old DB-25 parallel port, you can make one even simpler JTAG probe. But you'll have to provide your own 3.3V power supply. 
About JTAG probe, I suggest this one: https://raw.githubusercontent.com/fmarkova/CC-programmer/master/Firmware/JTAG-Simple.jpg
Just connect VCC lines to your own 3.3V power supply, and make shared ground. 
NOTE: You need 74HC244 or equivalent, in order to translate 5V to 3.3V signals!
NOTE: I'm not responsible for any potential hardware damage or data loss if you're using this method!
