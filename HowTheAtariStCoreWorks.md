This document is not updated as often as the HDL code itself. Some information may thus be outdated.

# The Basics #

The Atari ST core for the MIST board is a hardware description of the entire hardware of the original Atari ST. This is unlike an emulation which consist of code making one CPU behave like a different one. Instead the hardware description is used to generate a configuration for a field programmable gate array (FPGA) which in turn behaves like the configuration tells it. The resulting device is not an emulation. Instead during configuration the FPGA really becomes the hardware described in the configuration.

Like 99% of all other FPGA boards, the MIST board mainly consist of a FPGA, Memory, a clock source and various external interfaces (Video, audio). The MIST board additionally includes a Microcontroller (IO controller) connected to the FPGA and further interfaces (USB, SD card, joysticks).

A FPGA hardware configuration re-implementing a computer typically consist of some very basic function blocks:
  * A CPU core
  * A Memory controller
  * IO

## CPU ##

In the case of an Atari ST, the CPU core needs to be the hardware description of the Motorola 68000 CPU the original Atari ST used. Several such cores exist, e.g.:
  * TG68 (http://opencores.org/project,tg68)
  * ao68000 (http://opencores.org/project,ao68000)
  * Suska 68000 (http://experiment-s.de/en)

The first Atari core for the MIST included the TG68 as it was already used in the Minimig project to replicate an Commordore Amiga. However, some features required by the Atari ST, but not the Amiga were not present in that core (e.g. the bus error handling). Furthermore the "TG68 wrapper" that came with the minimig port has been removed. The Atari core thus now uses the bare TG68K core.

A second Atari ST core uses the ao68000 core.

## Memory controller ##

The memory controller is very board specific. It needs to be able to address the RAM chip(s) used on the board in question. On the MIST board a single 16M\*16bit SDR SDRAM is being used giving a total of 32 MBytes. This is a RAM chip very similar to the one use on the Terasic DE1 board. Thus the memory controller from the Minimig port of the minimig de1 port (https://github.com/rkrajnc/minimig-de1) was initially being used. Later this sdram controller has been removed and replaced by a much simpler one providing only exaclty those functionality required by the Atari ST core.

## IO ##

The CPU core together with the memory controller and some glue logic already provide a working computer. But in order to get some input and output from the board various IO hardware needs to be implemented. This is where it gets Atari or Amiga specific. The Atari ST core includes descriptions of all the atari specific IO chips (mmu, glue, shifter, mfp68901, acia, dma, fdc, ...).

# The Details #

## Clocks and Bus Timing ##

A original Atari ST ran at 8MHz. This means that the CPU ran at a 8Mhz clock signal. This 8Mhz clock was derived from a 32MHz clock source. Various other frequencies (16Mhz, 2MHz) are also derived from this and were used in different places in the ST (video used 32Mhz, audio used 2MHz, ...). The Atari ST was able to do one 16 bit RAM transfer within 250ns effectively requiring two 8Mhz cycles per RAM transfer cycle. One of these cycles was used by the CPU and another one by the video controller (shifter+mmu). A complete system cycles thus took 500ns (2 millions system cycles per second). This matches the fact that the 68k CPU itself also typically requires four 8Mhz clock cycles to perform one external bus transfer.

The Atari core tries to mimic this behaviour. The board itself runs on 27Mhz. This clock is not used in the Atari ST core itself. Instead the 27Mhz is fed into a PLL which generates various clocks of 128Mhz, 32Mhz and 2.4576Mhz. Just like in a real ST the 32Mhz is used to generate further clocks incl. the 8Mhz clock for the system cycle generation.

In the Atari core the system cycle also is 500ns (2MHz). This includes four seperate 8 MHz bus cycles. In plain ST mode one of these cycles is used for the video generation and one for the CPU. The other two cycles are unused. In Mega STE 16 MHz mode another cycle is assigned to the CPU.

## Video ##

The machines from the age of home computers usually used oridinary TV sets for their video output. The Atari ST used this for the color video modes.  The monochrome video mode used a special high resolution screen. The video signal timing such a computer generated is typically not compatible with todays computer displays.

The Atari ST monochrome video mode with 640 x 400 Pixels at 72 Hz refresh rate used a special high resolution screen. Fortunately, this timing still is compatible.  So the Atari core for the MIST generates a monochrome signal that is quite close to the original Atari ST monochrome timing. But for the color modes this doesn't work.

The first problem with the tv timing of the color modes is that the horizontal scan frequency of 15.75kHz is way to low for today's computer screens which usually expect this frequency to be at least 30kHz. One solution for this is to use an approach known as "scan doubling" which simply outputs each line twice.  The resulting horizontal scan frequency is 31.5kHz which is acceptable to any modern computer screen or TV set with VGA computer input. The Atari core for the MIST does this by implementing ram buffers inside it's implementation of the Atari shifter video chip. Two of these buffers exist and while one line of video data is being read from memory the other one is displayed twice from the buffer at double speed. The resulting video modes generate twice the hsync pulses per image as a real Atari ST and also the number of output enable events is twice their number on a real ST per frame. Since these signals can be evaluated by software fake signals have to be generated resembling the original ST timing of their original counterparts to let software use these signals at the correct speed to e.g. display more than standard 16 colors at once.

Still a second problem remains: The vertical scan frequency is 50Hz in PAL/50Hz mode. This is lower than the 56Hz required by many modern VGA screens which demand at least 56Hz. Still some screens cope with 50Hz. In NTSC mode (when e.g. using an amercian TOS) the screen refresh rate is 60Hz. This is directly compatible with most VGA screens.

The Atari ST core provides a switch via the on-screen-display (OSD) to allow a  50Hz output or to tweak the 50Hz signal into a 56Hz signal. The downside of this is that this frequency is also available as a signal to generate software timings and some software may not work properly with 56Hz instead of 50Hz. The same video timing is used for both color video modes (ST mid 640x200 and ST low 320x200).

The Atari ST core on the MIST thus supports four video timings:

  * monochrome 640x400 at 72Hz (HIREZ)
  * color 640/320x200 at 60Hz (NTSC),
  * color 640/320x200 at 50Hz (PAL), and
  * color 640/320x200 at 56Hz (modified PAL).

The shifter implementation currently does not implement the undocumented features of the shifter. This primarily means that ["opening the borders" or "software overscan"](http://dss.in.tum.de/files/brandt-research/fullscreen.txt) does not work. This is mainly used in demos and intro screens and games and normal applications are not affected by this. Adding this feature to the MIST shifter would be a little futile at the moment as this also requires a 100% exact CPU timing which all of the existing HDL implementations of he 68k CPU do not provide. Again, games and normal applications are usually not affected by this.

It is technically possible to do a perfect 68k timing and a perfect shifter inside the MISTs FPGA (or any other FPGA). This has just not been done so far.

### Misaligned video ###

When adapting the original video timing to the VGA video timing one can decide either to to stay as close to the original timing as possible to lower the chances of incompatibility with software expecting a certain timing. Or on the other hand it's possible to create a video timing as similar as possible to a well-known VGA video timing to lower the chances of misaligned video on a standard VGA screen.

Unfortunately is not easily possible to meet both goals. The only way to achieve this is to capture the entire video frame using an Atari ST compatible timing into a separate buffer memory and to replay it from there using a standard VGA timing. This approach is very complex, requires many FPGA resources and has not been implemented in the MIST.

The MIST Atari core instead is as close to the original Atari ST timing as possible. As a result the video may not be a perfect pendant to a typical VGA signal and thus a VGA screen may not be able to e.g. centre the screen perfectly or to make use of the entire screen estate.

# IO Controller #

The IO Controller has two main jobs:
  * Configuration of the FPGA ("booting")
  * IO

The main reason for the IO controller is that it provides a convenient way of configuring the FPGA. Typically FPGAs are configures using small flash memories. These are small and cheap, but they need a special hardware to be flashed  themselves and there's no user friendly way for updates.

The MIST therefore uses a microcontroller to configure the FPGA using a file stored on SD card. This way the FPGA configuration can easily be updated just by replacing a file on the SD card. Multiple configurations can be stored on different SD cards.

And now that a IO controller is present on the board it can be used for other purposes as well. One of these is to use the SD card to emulate floppy disk drives and hard disks.  In case of the MIST board (and unlike most other FPGA boards of this kind) the IO controller also interfaces to mice, keyboards and joysticks. This is for two reasons:

  * The MIST uses USB which is a very complex protocol which is easier to implement in software on a microcontroller than in hardware on an FPGA
  * The microcontroller has 5V tolerant inputs and can directly be connected to classic Atari ST joysticks which the FPGA can't as it is not able to directly interface to 5V inputs

## Connection between the FPGA and the IO controller ##

The connection between the IO controller and the FPGA is [SPI based](http://de.wikipedia.org/wiki/Serial_Peripheral_Interface). The IO controller is the SPI master and the FPGA is the SPI client. There is a total of 4 SPI select line available between both allowing for four SPI channels. Currently three channels are in use:

  * Memory IO
  * User IO
  * OSD

### Memory IO ###

The memory io SPI channel gives the IO controller direct access to the cores memory. This is primarily used to upload ROM images into the cores memory. It is also used to do draw boot messages into the cores video memory.

The memory IO channel also containers a 32 bit system control register. This is used by the IO controller to control the cores reset signal and to change system parameters (CPU config, memory size, video modes, floppy write protection, ...)

Floppy (and in the future hard disk) emulation is also done via this channel. The IO controller requests the state if the flopppy and dma controllers via the memory interface and up- or downloads data if necessary.

### User IO ###

The user IO channel is used to forward keyboard, mouse and joystick events to the core. For the Atari ST, the IO controller implements the ikbd protocol and communicates directly with the acia inside the FPGA configuration.

### OSD ###

The on-screen-display (OSD) is controlled via the OSD channel. This channel is compatible to the one of the original Minimig and is used in the Atari core as well as the Minimig Amiga core.

The Atari ST core only implements the display specific parts while the Amiga/Minimig core also uses this to transfer OSD related keyboard events and OSD related system control signals.