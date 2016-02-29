The MIST board is a work in progress. All code and all tools required to work on the MIST board is free.

# Development hardware #

The only thing you'll unfortunately have to pay for is the board itself which we've tried to keep as cheap as possible. But the board itself is a must. There are no simulators or similar for the board.

See TheBoard for a detailed view of the connectors being used.

Some additional hardware may be handy during development:

  * A "ttl uart usb" converter can be connected to the 4 pin header SV3 right to the SD card slot. This serial port carries debug output of the ARM IO controller at 115200/8N1
  * A ARM JTAG cable (tested with Olimex ARM-USB-Tiny-H https://www.olimex.com/Products/ARM/JTAG/ARM-USB-TINY-H/) can be connected to SV4 to be used to flash the ARM controller directly speeding up turn around times significantly
  * A Altera Byte Blaster USB cable (tested with various cheap china clones, although some of them only worked under Windows) can be connected to SV1 allowing for fast FPGA upload and advanced FPGA debugging

For more information on using a Byte Blaster see UsingAByteBlaster.

![http://mist-board.googlecode.com/svn/wiki/adapters.jpg](http://mist-board.googlecode.com/svn/wiki/adapters.jpg)

# IO controller vs. FPGA development #

There are two main devices that drive the MIST board:

  * The IO controller is a normal ARM CPU and is running ordinary software. It is mainly used for for IO (hence the name) and e.g. talks to the USB ports as well as the SD card and the joysticks
  * The FPGA is doing 90% of the magic and implements the hardware of the targets

As a rule of thumb: Working on IO features mainly happens on the IO controller. Examples are:

  * Support for new USB devices (e.g. game pads and memory sticks)
  * Support for new SD card types, new file systems etc
  * Keyboard mappings
  * Mouse settings
  * On Screen Display (except the OSD video signal generation itself)

All issues regarding the target core happen on the the FPGA. Examples are:

  * Fixing of target bugs (e.g. mis-implemented hardware preventing certain software from running)
  * Video display
  * Audio output
  * Additional target features (e.g. new target chipsets like STE/AGA)
  * Speedups (Memory caches, fast ram interfaces, faster target CPU)

And there are things that affect both componentes equally. Examples are:

  * Floppy and harddisk emulation
  * FPGA configuation

# Firmware development #

![http://mist-board.googlecode.com/svn/wiki/firmware.png](http://mist-board.googlecode.com/svn/wiki/firmware.png)

# Core development #

## Debugging the core ##

Signaltap:

![http://mist-board.googlecode.com/svn/wiki/signaltap_1.jpg](http://mist-board.googlecode.com/svn/wiki/signaltap_1.jpg)

![http://mist-board.googlecode.com/svn/wiki/signaltap_2.jpg](http://mist-board.googlecode.com/svn/wiki/signaltap_2.jpg)