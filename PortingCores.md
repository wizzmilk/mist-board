The MIST is not the first FPGA board and not the first FPGA
project. [ListOfFPGAProjects](ListOfFPGAProjects.md) lists FPGA based projects that may be
interesting for the MIST board as well. The effort of porting depends
very much on the complexity of the project to be ported.

For further details regarding the pins being used on the FPGA refer to
[Pins](Pins.md).

# The simple case: FPGA only projects #

Projects only using a single FPGA without any additional hardware are
very simple to port. The only things you have to take care of is the
fact that the MIST uses a single 27MHz clock source only. You might
need to use a PLL inside the FPGA to adjust to the clock requirements
the project you are going to port demands.

For simple tests the LED output of the FPGA may be used.

# Audio and Video output #

A single LED is usually not sufficient for output. The MIST board
provides 3\*6 bit video output via VGA and 2 outputs for audio. Many
FPGA setups use similar outputs and many such projects can directly be
ported to the MIST by just specifying the correct pins. It's even
possible to output other signals than VGA on the VGA port. E.g. a
analogue RGB TV signal may be created there and be used with a
simple VGA to SCART cable like the "hama 43192".

The two audio outputs are connected to a low pass filter stage so that
pwm signals can be used to create analogue signals.

# SDRAM #

The EP3C25 FPGA of the MIST provides slightly more than 64kBytes of
internal memory. You can use major parts of this as RAM and ROM for
your design. Small devices like the Sinclair ZX81, the Commodore
VIC-20 or the Pacman arcade machine can be implemented without using
any external memory. For designs demanding more memory the external
32MBytes SDRAM of the MIST has to be used. This is a similar chip to
the 8Mbyte one being used on the Terasic DE1 board and HDL code
written for that board can directly be used on the MIST board as well
(SDRAM\_A[12](12.md) has to be tied to GND then).

If ROM images bigger than a few kilobytes are to be used a means to
upload ROM contents from SD card into SDRAM is needed. The Atari ST
core provides an example for that.

Many 8 bit cores come with various variants of SDRAM
controllers. Examples can be found in the SVN repository under
https://code.google.com/p/mist-board/source/browse/#svn%2Ftrunk%2Fcores

# Input peripherals - SPI / IO Controller #

It's likely the the core in question uses input devices like a
keyboard, mouse or a joystick.

The FPGA on the MIST board is not directly connected to any
input device. Instead it is connected via a SPI channel to the ARM IO
Controller which in turn is connected to the SD card as well as the
USB and joystick ports. The firmware of the IO Controller handles all
IO and sends events into the FPGA via SPI. The FPGA core needs to
provide a ID code via SPI so the IO Controller knows how to
communicate with the FPGA. Currently five modes are implemented:

  * Atari ST mode for floppy/hdd emulation and mouse, joystick and keyboard io and OSD
  * Amiga/Minimig mode for floppy/hdd emulation and mouse, joystick and keyboard io and OSD
  * PACE mode for pure digital joystick IO
  * Dumb mode for no IO at all
  * 8bit mode for classic 8 bit computers and consoles

Especially the 8 bit mode is designed for porting existing cores from
other platforms. It is being used for over 8 cores now (incl. Sega
Master System, Atari 2600, Atari XL, ...)

## Legacy device wrapper user\_io.v/sd\_card.v ##

There's a de-facto standard for certain legancy input devices used on FPGA
boards. Basically all of them connecting keyboards use PS/2 keyboards. And
most of them requiring mass storage use SD cards and joysticks are often
simple digital Atari style joysticks.

The MIST provides so-called legacy wrappers for these making it as
easy as possible to run such cores on the MIST board. These wrappers
consist of two parts, a firmware parts placed inside the IO controller
and a HDL part placed in the core. The latest official firmware
already includes support for this and you can take the HDL part from
one of the many example projects. Look out for a file named user\_io.v
in the MIST SVN.

### Joysticks ###

The primary input for a gaming oriented platform like the MIST sure is
the joystick. The user\_io.v gives you access to up to two classic Atari/C64 style digital joysticks. Each joystick is reported via a 8 bit field which consists
of bits for right, left, down, up, and four fire buttons beginning with
the least significant bit. The bits return '1' if the related button or
direction is activated.

The primary joystick is joystick 1 (not joystick 0!!). This is caused
by the fact that joystick port 0 is often used to connect mice on
16 bit systems like Amiga and Atari ST and the joystick is typically
used in port 1 there.

The MIST board contains two classic DB9 connectors for classic Atari/C64
style digital joysticks. One port is used for joystick 0, one is used
for joystick 1.

USB joysticks are also supported. They are inlemented transparently and
the core developer doesn't have to care about them at all. Once a USB
joystick is detected it becomes joystick 1 (the new primary joystick!)
and the former joystick 1 becomes joystick 0 and joystick 0 becomes joystick 2. If a second USB joystick
is attached it becomes joystick 0 (the secondary joystick) and the
physical DB9 ports become joysticks 2 and 3. In total up to four joysticks with up to 4 buttons each are supported. At least 2 USB joysticks need to be used for that.

Later versions of user\_io.v additionally support analog joysticks. Two
axes for each joystick are reported in a 16 bit field. The upper 8
bits are the X axis, the lower 8 bits are the y axis. Both return
their position as signed 8 bit values ranging from -128 to 127. This
is only useful for USB joysticks as DB9 joysticks are pure
digital. However, digital joysticks (USB and DB9 ones) are also
reported through the analog interface although they only report
min/center/max positions using the values -128, 0 and 127.

Joysticks can be emulated using the keyboard. If you press the num
lock key you can cycle through four states indicated by the num lock
and scroll lock LEDs. If both LEDs are off no emulation takes
place. This is the default state.

If you press num lock once you'll enable emulation of joystick 0 as
indicated by the fact that only the num lock led lights up. The cursor
keys as well as the left Ctrl, Shift, Alt and Windows keys are now reported as
joystick events instead of keyboard events. This is totally transparent
to the core and no special support is required for this in the core.

If you press num lock a second time only scroll lock lights up and the
keyboard is used to emulate joystick 1.

If you press num lock a third time both leds are lit indicating that
the device is in mouse emulation mode.

A fourth press on num lock return to the default state.

### Keyboards ###

Many existing cores expect a direct interface to a PS2 keyboard.  The
MISTs legacy wrappers include a PS2 keyboard emulation. Any USB
keyboard connected to the MIST is translated by the wrappers into PS2
and can directly be connected to any core expecting a PS2 keyboard.
The user\_io.c needs to be provided with a 12-16kHz (Kilohertz!!) clock
since PS2 keyboards communicate at this speed.

Special care has to be taken for the F12 key and the num lock. These
are used by the IO controller to open its on screen display (OSD) as
described further below or for the aforementioned joystick
emulation. Therefore these keys are usually not forwarded to the core
as also explained below in the OSD section.

Most keyboards have LEDs for Caps Lock, Scroll Lock and Num
Lock. These are controlled internally by the IO controller and are not
available to the PS2 emulation.

### Mice ###

Mice are supported through a legacy wrapper as well. On FPGA side this
is technically the same as the PS2 keyboard wrapper. The only difference
is that the IO controller uses this to send PS2 mouse data instead
of keyboard events.

Currently only a classic two button mouse is implemented.

This mouse is e.g. being used by the OneChipMSX core.

### SD cards ###

The latest user\_io.v comes with an SD card implementation which
behaves like an SD card to the core and in the background communicates
with the Io controller to request IO from the physical SD card
connected to the IO controller.

The SD card implementation supports SD as well as SDHC (high capacity
cards > 2GB). But man cores don't support SDHC. The legacy wrapper
(sd\_card.v) has an input signal named allow\_sdhc which indicates that
the core copes with SDHC card. Set this to '1' if your core supports
SDHC and to '0' otherwise. This allows some magic to be performed by
the wrappers. For cores that don't support SDHC the wrapper will
behave like and SD card even if and SDHC card is physically inserted.
This will work as long as all data being accessed is in the first 2 GB
on the card. If it isn't accesses will be messed up and wrong data is
returned. But since write access to the card is completely forbidden
in this case there's no risk of data corruption. A warning message is
additionally displayed in the screen if the OSD is implemented.

The SD card implementation is not complete. It lacks the multiblock
commands and can only read and write single sectors. Contact us if
you need multiblock commands.

Implemented commands are:

  * CMD0 - GO\_IDLE\_STATE
  * CMD1 - SEND\_OP\_COND
  * ACMD41 - APP\_SEND\_OP\_COND
  * CMD8 - SEND\_IF\_COND
  * CMD9 - SEND\_CSD
  * CMD10 - SEND\_CID
  * CMD16 - SET\_BLOCKLEN
  * CMD17 - READ\_SINGLE\_BLOCK
  * CMD24 - WRITE\_SINGLE\_BLOCK
  * CMD55 - APP\_CMD
  * CMD58 - READ\_OCR

All other commands return code 4 "command not implemented".

This wrapper component is rather complex and new. Please expect
problems and report them back.

### On Screen display (OSD) ###

Many projects use permanent on board storage like flash memory to
store game cartridge images or the like. The MIST board does not have
any permanent memory in board.

Instead it provides a simple means of uploading data like cartridge
images from SD card. Cores like the Atari 2600 and the Sega Master
System use this. But also the TX81 and ZX spectrum use this to upload
tape images and the C64 core can use the same mechanism to inject PRG
files directly into C64 memory without any tape or floppy emulation.

The user controls this feature via the on screen display (OSD) this is
a simple verilog component named osd.v which is simply integrated into
the video data path and does the OSD painting without any further
support in the core. Everything is done inside the osd.v and the
firmware of the IO controller.

Once the OSD is properly integrated the IO controller will let the
user open it via the F12 key and provide additional functions though
it like the ability to save settings, change the running core or flash
a new firmware.

See section "config string" below for more details.

### Cartridge upload ###

Projects allowing direct cartridge upload using the OSD usually
implement this using a file named data\_io.v. Different versions of
this file exist as some cores upload the cartridge data into FPGA
internal block ram (e.g. Atari 2600) or in SDRAM (e.g. Sega Master
System). A signal indicates that the upload takes place and can be
used to e.g. halt the CPU during that time or trigger a reset
afterwards.

### The status byte ###

The IO controller provides a "status bytes" via user\_io.v. The lsb of
this is set to 1 for a few milliseconds whenever the IO controller
reboots. It's good practice to also reset the core then. Thus
status(0) is connected to the internal reset in nearly all cores.
The other 7 bit are user defined as explained below.

The status byte can be saved to SD card through the OSD and is
restored whenever a core of the same name is reloaded.

### Config String ###

The IO controller knows nothing about the running core. Therefore a
"config string" can be provided via the user\_io.v by the Core to for
use by the IO controller. Many examples for this exist in the MIST SVN
in VHDL as well a verilog.

The string consists of several ASCII entries being seperated by the
Semicolon. First part of the String is the name of the core. This is
used in the OSD as well as for reading and writing config files
from/to the the SD card also using the OSD.

To be useful a valid config string requires the OSD component to be
used. If no OSD is to be implemented the config string should be left
empty so the IO controller knows that no OSD is present and that e.g.
the F12 key doesn't habe to be captured and can be forwarded to the core
for its own use. The Atari Xl core does it that way.

The second parameter in the config string is the file extension for
image files to be used with the "Cartridge upload" mechanism as
described previously.

Further OSD entries can be integrated in the config string. Currently
two types of options are supported: "Toggles and Opetions".

Toggles can be used like push buttons and just consist of a single
text entry. Selecting a toggle in the OSD sets a bit inside the status
byte to '1' for a few milliseconds. This can be used to implement a
Reset button via the OSD. An example string would be "T1,Reset" which
would toggle bit 1 in the status byte whenever the Reset entry in the
OSD is selected.

Option bits have a more complex syntax and can be used like physical
switches. They have a label and two strings that are displayed
alternally whenever the Option is changed. This can e.g. be used to
implement a means to switch between PAL and NTSC video. An exmaple for
this would be "O2,Video,PAL,NTSC" which would allow to switch between
PAL and NTSC which is reflected to the core through bit 2 in the
status byte. The default setting is 0, so in this case it's the PAL
video mode by default. But changes can be saved as explained in the
status byte section.

A full config string may look like this:

SMS;SMS;O1,Video,NTSC,PAL;O2,Joysticks,Normal,Swapped;T3,Pause;T4,Reset

The platform name is SMS and so is the file name extension when uploading
cartridge images. Option 1 allows to toggle between PAL and NTSC video,
option 2 allows to swap the joysticks. Toggle 2 acts the a pause button
and toggle 4 like a reset button. This is the config string of the
ega Master System.

### Code examples ###

Various variants of the HDL model for the legacy device wrappers can
be found in the ported projects in the MIST SVN at
https://code.google.com/p/mist-board/source/browse/#svn%2Ftrunk%2Fcores.
Examples are the ZX01 and ZX spectrum cores which use the PS/2
keyboard emulation.

The SDBootstrap demo from https://github.com/robinsonb5/ZPUDemos uses
the SD card implementation.

The files in question are usually named user\_io.v, osd.v, data\_io.v and
sd\_card.v

# Expansion Pins #

The FPGA has two unsed expansion pins UART\_TX and UART\_RX. These are
currently used by the [MIDI interface](MidiAddon.md). Cores that require
more complex additional hardware like e.g. cartridge ports typically
cannot be ported directly to the MIST unless the hardware in question
is fully implemented inside the FPGA which is e.g. possible for ROM
cartridges.