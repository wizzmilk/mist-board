# What is this? #

Short answer: This is a configurable device that's able to mimic the entire hardware of the computers of the 80's and early 90's. For more details read WhatIsThis

# Is this an emulator? #

No. An emulator is a software that recreates the behaviour of one machine on another one. The MIST (like all other FPGA based approaches) re-implements the hardware or at least implements a hardware that behaves like the original machine.

# So this behaves 100% identical to the real thing? #

No. One reason is that the hardware of the original machines and the chips used inside them is not fully documented. Another reason is that the support for modern peripherals (VGA screen, USB keyboards, ...) requires changes to the behaviour of the original machine that's slightly incompatible. E.g. a VGA screen is unable to display the video signal of a genuine Atari ST. Thus any re-implementation meant to drive a VGA screen cannot behave 100% like the original device.

# What level of compatibility can i expect? #

You can expect over 95% if the Amiga and AtariST games to work on the MIST. The remaining 5% use hardware features that are hard or even impossible to implement.

# What is supported by now? #

Currently cores exist that re-implement early computers of the Amiga family (A500-A2000) and early Atari ST's (520ST-MegaST4). These include some addtional support for memory expansions (up to ~ 14MB on Amiga as well as Atari ST) and some extra performance (currently for Amiga only).

Also the Pacman arcade machine has been implemented.

# What will be supported in the future? #

The FPGA size limits the size/complexity of the hardware that can be re-implemented. The FPGA of the MIST board was chosen to be big enough to implement the intended machines and cheap enough to allow for an affordable setup. As a result the limits are somewhere in the range of an Amiga 1200 or Atari Mega STE. The MIST comes with 32 MBytes RAM. Since the 68k CPU can only address 16 MB and kickstart or TOS ROM images are also stored in RAM the maximum RAM size for a re-implemented device is somewhere in the ranges of 14 Megabytes. It is possible to use the remaining 16 MB for special RAM expansions using a banking approach. This may be used for ramdisks or the like and needs special drivers on Atari or Amiga side.

# What about Amiga 3000 or Atari TT/Falcon? #

These both machines came with a 68030 CPU which is significantly more complex than the 68000 CPU the Amiga 500 or Atari ST used. It's very lilely that these cannot be implemented in the FPGA used on the MIST.

However, it may be possible to reach a speed similar to those machines. E.g. [this blog entry](http://retroramblings.net/?p=280) shows a re-implementation of an 68000 based Amiga being accelerated to the speed somewhere between a Amiga 3000 and Amiga 4000. The resulting device rechnically is not an Amiga 3000 and wouldn't support software that requires special features provided by the 68030 only. But it's at least as fast as one.

# Can i attach legacy device xyz? #

Although FPGAs can be used to implement nearly any type of hardware interface, some support hardware is usually needed to adopt to the physical interface itself. E.g. voltage levels need to be adopted from the 0-3.3V range supported by an FPGA to the +/-12V range used by RS232 if such an interface is to be implemented.

The only legacy devices currently supported by the MIST are "C64 style" joysticks. A MIDI add-on is also available. Support for other hardware (ROM cartridges, printers, Atari Mice, original Atari screens, ...) are not planned.

It is often possible to implement the peripheral inside the FPGA. This is true for e.g. ROM cartridges and even dongles but also for graphic cards or memory expander cards.