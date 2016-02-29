# Already implemented #

  * 8Mhz CPU for ST and STE modes
  * 16Mhz CPU for Mega STE mode
  * **~48MHz CPU** for STEroids mode, roughly as fast as a TT
  * Up to **14MB** fully DMA supported and video capable ST RAM
  * ROM cartridge support
  * Support for 256k and 192k ROM images
  * TOS versions 1.00, 1.02, 1.04, 1.06, 1,62 2.06 and EmuTOS 0.8.7, 0.9.0 and 0.9.1 and probably more ...
  * All three ST video modes
  * SM194/Viking compatible **1280x1024 high rez video mode**
  * Scan doubler for color modes for VGA compatibility
  * PAL 56 HZ for VGA compatibility
  * Floppy disk emulation via SD card (.ST formats incl. HD (1.44MB) and ED (2.88MB))
  * AHDI compatible ACSI/HDD emulation via SD card using Hatari compatible ACSI images
  * MFP (Timers, RS232 redirection, IRQs, ...)
  * Sound
  * IKBD emulation (joysticks, mouse, keyboard)
  * USB mice, keyboards and joysticks
  * Printer/RS232/MIDI redirection to a PC connected via USB
  * **Ethernec compatible network** using DLink DUB-E100
  * Minimig on screen display (OSD)
  * Ability to change the core (to e.g. boot into Minimig)
  * MIDI add-on board
  * Blitter (Optional for ST, mandatory for STE)
  * STE video (4 bit per color, hard scrolling, split screen, ...)
  * STE DMA sound

# Planned/Possible #

  * More extended video modes (e.g. 640x480 @ 16 colors, 1024x768 @ 4 colors)
  * 16MB fast/alternate RAM
  * Support for STX (copy protoected) floppy disk images
  * USB floppy disk drives, memory sticks and hard drives

# Frequently asked things that will most likely never happen #

  * Falcon compatibility (This requires a DSP and various other complex things that don't fit into the MIST FPGA)
  * 68040/68060 add-on board (the MIST doesn't have enough spare IO pins for this)
  * 68040/68060 FPGA implementation (the MIST FPGA is too small for this)

# Summary #

Mega STE and more has been done, parts of a TT are also done but full TT compatibility is very unlikely and Falcon (incl. e.g. DSP) is impossible