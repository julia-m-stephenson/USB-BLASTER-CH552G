# USB-BLASTER-CH552G
Reverse Engineering the Cheap USB-Blaster<br/>
<br/>
I bought one of the cheapest USB-Blaster clones from Aliexpress. I know, I'm cheap lol<br/>
![CheapPix](/images/CHEAPO.jpg)
Unfortunately it didn't support 5V and probably didn't work on 3V3 as the firmware is a bit suspect.<br/>
<br/>
The reverse engineered schematic is in folder usb_blaster_ch552g (see also usb_blaster_ch552g.pdf)<br/>
This also required adding a CH552G symbol to https://github.com/julia-m-stephenson/wch-kicad-lbr<br/>
<br/>
Find the data sheets in docs directory.<br/>
<br/>
I modifed the schematic so the board will work with 5V devices (only) see usb_blaster_ch552g_5V (and usb_blaster_ch552g_5V.pdf)
![CheapPix](/images/CHEAPO_5V.jpg)<br/>


Basically lift pin 15 and connect to F1 (5V) and remove the 3v3 regulator (U1). <br/>
Ignore the wire on PIN 16 that was a failed experiment. <br/>
This change runs the MCU on 5V and uses its internal 3V3 regulator (pin 16) to drive the LEDs and power for the 10K programming resistor.<br/>

Unfortunately this still didn't connect to the Atera Device I wanted to use - EPM7128SLC, EPM7064SLC.<br/>

I found some new firmware created by Doug (https://www.downtowndougbrown.com/2024/06/fixing-a-knockoff-altera-usb-blaster-that-never-worked/)<br/>

A copy is provided in directory firmware for your convenience.<br/>

The open source tools did not work for me but luckily the manufacturer provided tools worked great under Windows 10.<br/>
A copy of the WCH programming tool is in the tools directory along with a driver which I don't think you need.<br/>

I think the open source tools fail because the CH552G bootloader only listens for host traffic for a short time before jumping into the firmware already loaded.<br/>

To get this to work I followed these instructions from "KUKUS" yo will see them at the very bottom of Dougs's page linked above<br/>
NOTE: My 5V change keeps 3V3 on the programming header which is needed for the 10K resistor link.<br/>

They are :<br/>
KUKUS @ 2025-06-16 15:04<br/>
<br/>
Only for chinese CH552G Usb blaster clones:<br/>
https://www.aliexpress.com/item/1005006124786647.html<br/>
for Windows 10 users:<br/>
– install CH375_DRIVER (CH372DRV.EXE from WCH website)<br/>
-! driver CH372DRV.EXE can be renamed CH372DRV.CAB and files be extracted with Winzip/RAR etc.!<br/>
– open plastic case, add 10k resistor between D+ and 3V3<br/>
– install WCHISP_Tool3.3 or latest WCHISP_Studio (better!) (WCH website)<br/>
– if device not detected update driver from via Device_Manager using driver files from extracted .CAB<br/>
– make sure to select CH55x/USB chip model CH552<br/>
– Object File1 = load the usb_blaster.bin file here and check box on the right<br/>
– Make sure “Enable RST Pin as manual reset pin” and “Run the Taget Program …” are NOT checked !<br/>
<br/>
IMPORTANT!!<br/>
– ! Make sure “Automatic Download When Device Connect” is CHECKED !<br/>
– insert device on USB2.0 port.<br/>
– it will automatically download usb_blaster.bin file in device flash memory<br/>
– remove 10k resistor from D+ and 3v3<br/>
– Plug in USB => You now have a fully functional “Altera USB Blaster Device” 🙂<br/>
Tested and working in Quartus Standard 24.1 as per 2025 😉<br/>
<br/>
All files provided as is and I accept no resposibility for any damage caused to the fabric of the space-time continuum!<br/>
<br/>