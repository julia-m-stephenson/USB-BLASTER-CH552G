# USB-BLASTER-CH552G
Reverse Engineering the Cheap USB-Blaster

I bought one for the cheapest USB-Blaster clones from Aliexpress.
![CheapPix](/images/CHEAPO.jpg)
Unfortunately it didn't support 5V and probably didn't work on 3V3 as the firmware is a bit suspect.

The reverse engineered schematic is in folder usb_blaster_ch552g (see also usb_blaster_ch552g.pdf)
This also required adding a CH552G symbol to https://github.com/julia-m-stephenson/wch-kicad-lbr

Find the data sheets in docs directory.

I modifed the schematic so the board will work with 5V devices (only) see usb_blaster_ch552g_5V (and usb_blaster_ch552g_5V.pdf)
![CheapPix](/images/CHEAPO_5V.jpg)


Basically lift pin 15 and connect to F1 (5V) and remove the 3v3 regulator (U1). 
Ignore the wire on PIN 16 that was a failed experiment. 
This change runs the MCU on 5V and uses its internal 3V3 regulator (pin 16) to drive the LEDs and power for the 10K programming resistor.

Unfortunately this still didn't connect to the Atera Device I wanted to use - EPM7128SLC, EPM7064SLC.

I found some new firmware created by Doug (https://www.downtowndougbrown.com/2024/06/fixing-a-knockoff-altera-usb-blaster-that-never-worked/)

A copy is provided in directory firmware for your convenience.

The open source tools did not work for me but luckily the manufacturer provided tools worked great under Windows 10.
A copy of the WCH programming tool is in the tools directory along with a driver which I don't think you need.

I think the open source tools fail because the CH552G bootloader only listens for host traffic for a short time before jumping into the firmware already loaded.

To get this to work I followed these instructions from "KUKUS" yo will see them at the very bottom of Dougs's page linked above
NOTE: My 5V change keeps 3V3 on the programming header which is needed for the 10K resistor link.

They are :-
KUKUS @ 2025-06-16 15:04

Only for chinese CH552G Usb blaster clones:
https://www.aliexpress.com/item/1005006124786647.html
for Windows 10 users:
– install CH375_DRIVER (CH372DRV.EXE from WCH website)
-! driver CH372DRV.EXE can be renamed CH372DRV.CAB and files be extracted with Winzip/RAR etc.!
– open plastic case, add 10k resistor between D+ and 3V3
– install WCHISP_Tool3.3 or latest WCHISP_Studio (better!) (WCH website)
– if device not detected update driver from via Device_Manager using driver files from extracted .CAB
– make sure to select CH55x/USB chip model CH552
– Object File1 = load the usb_blaster.bin file here and check box on the right
– Make sure “Enable RST Pin as manual reset pin” and “Run the Taget Program …” are NOT checked !

IMPORTANT!!
– ! Make sure “Automatic Download When Device Connect” is CHECKED !
– insert device on USB2.0 port.
– it will automatically download usb_blaster.bin file in device flash memory
– remove 10k resistor from D+ and 3v3
– Plug in USB => You now have a fully functional “Altera USB Blaster Device” 🙂
Tested and working in Quartus Standard 24.1 as per 2025 😉

All files provided as is and I accept no resposibility for any damage caused to the fabric of the space-time continuum!