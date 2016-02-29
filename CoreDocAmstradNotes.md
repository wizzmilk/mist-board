## Introduction ##

In this wiki page, you'll find all the Amstrad MiST Core development strategy : deployment of FPGAmstrad project on this lovely MiST-board final FPGA platform.

In FPGAmstrad's page http://www.cpcwiki.eu/index.php/FPGAmstrad, you'll find all about game bug localisation, and a TODO-list.

From Markus Hohmann homepage http://cpc.devilmarkus.de, you can try others games, playing them directly from your internet browser :)

# From Xilinx to Altera schematics #
For me, global schematics are really important, for developing and for deploying.
A schematic developed in order to be comparable to original documentation schematic is nice. FPGAmstrad is composed of 3 schematics :
  * amstrad\_motherboard : comparable to original Amstrad schematic.
  * amstrad\_video : does manage a true VGA output, using a internal VRAM.
  * bootloader\_sd : sdcard bootloader, in order to load ROM and dsk at boot, from sdcard.

As Xilinx schematics are not compatible with Altera, I do generate "vhf" files, and rename them :
  * FPGAmstrad\_amstrad\_motherboard.vhd
  * FPGAmstrad\_amstrad\_video.vhd
  * FPGAmstrad\_bootloader\_sd.vhd

And then I make a global schematic in Altera, that contains the previous components, and several MiST-board controlers :
  * FPGAmstrad\_amstrad\_motherboard.vhd
  * FPGAmstrad\_amstrad\_video.vhd
  * FPGAmstrad\_bootloader\_sd.vhd
  * sdcard.v
  * user\_io.v
  * data\_io.v
  * sdram.v
  * osd.v

I create then a main clock component, generating all clocks I want even the not clocks (a good practice, while using "time constraints"),
I also add some adapters, about wire/bus range solving :
  * MIST\_SDRAM.vhd : each SDRAM has a different RAM bus size
  * MIST\_DQM.vhd : just a small wiring helper
  * MIST\_RGB.vhd : each VGA output has different count of colors
  * MIST\_STATUS.vhd : mapping status wires
  * CONF\_STR.vhd : generating OSD parameter

# Xilinx to Altera #
While generating vhf files from Xilinx Schematics, a lot of small components have to be adapted :
  * INV component became 'not' instruction
  * AND2 component became 'and' instruction
  * OR2 component became 'or instruction
  * GND component became '0' value
  * VCC component became '1' value
The internal RAM, sync and async (with one or two clocks) are to adapt, I use mem\_altera\_gen.vhd for this purpose.

# Special things done during deploy #

sdram.v is personalized in order to solve address **after** the write or read event. It is due to Amstrad that permit writing in RAM hidden inside ROM : if I read I read ROM, if I write I write RAM.
sdram.v is also personalized in order having a clkref lower than 4MHz.

# RAM optimization #

Some efforts done about internal RAM.

As I could not do enter my 32KB VRAM, I used a 16KB+8KB+4KB VRAM, to display the 640x480 output, scanning my 800x600 VRAM : bottom of my VRAM is useless for a 640x480 display.

At this step, I have 0KB of internal RAM free. Now let's do appear 16KB more in order to deploy fully my FPGAmstrad project !

RAM inferred : [in Altera reg is inferred into RAM-block](http://quartushelp.altera.com/13.0/mergedProjects/hdl/vlog/vlog_file_dir_ram.htm) ([more](http://quartushelp.altera.com/13.0/mergedProjects/hdl/vlog/vlog_file_dir.htm)), so a reg written like this :
```
reg [7:0] dir_entry_reg [31:0]
```
became RAM-block.

In data\_io.v : dir\_entry\_reg does use /**synthesis noprune**/ in order to be not unwired. If I remove output dir\_entry\_d, RAM-block is inferred. If I let output dir\_entry\_d, LOGIC-block is inferred.

So I removed output dir\_entry\_d and set :
```
(* ramstyle = "logic" *) reg [7:0] dir_entry_reg [31:0] /* synthesis noprune */;
```
So I continue winning my 1KB internal RAM-block (here 256 Bytes was needed and turn into inferred LOGIC-block)

In sdcard.v
```
reg [7:0] buffer [511:0];
```
is a big reg and really important one (speaking to ARM SPI !), so I let it inferring into RAM-block.
But about cid and csd I does :
```
(* ramstyle = "logic" *) reg [7:0] cid [15:0];
(* ramstyle = "logic" *) reg [7:0] csd [15:0];
```
Winning 2KB of internal RAM for this small 128Bytes reg :)

In VRAM\_Palette, I had 16KB. But in fact a raster line is a 2+16+1 RAM palette line, so each line I store 19KB, so in fact 19\*600/2=5700 bytes (800x600 VRAM in fact thruly 800x300). So only a 8KB RAM palette only was needed in FPGAmstrad project finally.

At this step I won 10KB of internal RAM (I need 6KB more to succeed in my full FPGAmstrad deployment)

I can nibble 2KB more at end of RAM palette, that's what I does.
Now I have 12KB of internal RAM free :)

And 4KB in VRAM :
800x600=100\*300=30KB full
800x480=100\*240=24000 24000-16384=7616<8KB=8192
so VRAM with a start vertical offset can be composed of 16KB+8KB only. So I won my last 4KB here.
I did patch my simple\_GateArrayInterrupt component by parametering vertical offset :
```
 GA_interrupt : simple_GateArrayInterrupt
		generic map (VRAM_Voffset=>38*8-30*8-4*8+4  +0      +15) -- MiST +15 ?
```
I also patched my aZRaEL\_vram2vgaAmstradMiaow component by parametering vertical offset :
```
  XLXI_476 : aZRaEL_vram2vgaAmstradMiaow
		generic map (VOFFSET_NEGATIF =>0, -- MiST 0
		VOFFSET_PALETTE=>0) -- MiST 0
```

Now I can use my 16KB free RAM in VRAM double buffer. Reaching a full FGPAmstrad project deploy on MiST-board, unlocking others games : it is what is done in realise 002 of Amstrad core. I tested ChaseHQ does now run fine.

# Source code #
[BuildYourOwnZ80ComputerMiST\_v0.49.4.exp.25-r003.10-cafe](https://drive.google.com/file/d/0B-5y_zO_t-TnWDA5bEZzRkxFamc/view?usp=sharing)

Compiling OK in Quartus II 13.0 (Altera IDE), and a few in ISE Design Suite 14.7 (Xilinx IDE) - I have to report back some modifications from my deploy platform(Altera MiST-board) to my dev platform (Xilinx NEXYS4 from Digilent Inc.)

# Future #
Next actions are same as FPGAmstrad's TODO-list.

# Return #
  * A lot of games and demos were unlocked since I personalized the SDRAM controler, all about that will be reported back into FPGAmstrad project.
  * PWM was greatly ameliorated during this deploy, all about that will be reported back into FPGAmstrad project.
  * A small effort was done about interrupt cycles during this deploy, unlocking some demos, all about that will be reported back into FPGAmstrad project.
  * This project do compile in Xilinx and in Altera, but some files have to be edited each time I switch. That has to be improved.