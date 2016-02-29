# TG68 and SDRAM #

This page tries to explain how the TG68 CPU core and the sdram controller are cooperate inside the Minimig/Amiga and MIST/Atari
core. These parts are not board specific and apply to any other setup derived from TobiFlexx's Fampiga setup for the Chameleon 64
board. This includes boards like the MIST, the Terasic DE1 and DE2, the Chameleon 64 and the Retroramblings board.

# TG68K wrapper #

All these setups use more or less the same wrapper around the TG68KdotC kernel. This wrapper implements many of the external signals of a real 68k CPU (e.g. dtack). It also implements the amigas autoconfig (which is removed from the wrapper in the Atari ST setup) and generates signals and timings for different ram access types.

# RAM access types #

The Amiga as well as the Atari ST used the same basic principles to access RAM although the naming is different. Both support two RAM access types: Chip RAM/ST RAM and Fast RAM/Alternate RAM.

## Chip RAM / ST RAM ##

The so-called chip ram (on Amiga) or ST RAM (on Atari ST) is accessible not only by the CPU but also by parts of the chipset. This is used to implement Video RAM accesses as well as DMA functions. The Amiga mainly used DMA for video memory modification (copper, blitter) while the Atari ST mainly used DMA for mass storage transfers (floppy and harddisk). Later version of the Atari ST also implemented DMA hardware for video modifications (blitter) and audio output (STE DMA sound).

In both machines the 68000 CPU was running at a similar CPU clock speed of 7 (Amiga) or 8 (Atari ST) MHz. The memory access rates of ~500ns required by a 68000 CPU running at this speed was about half the access rates of 250ns typical DRAMs at that time offered. This fact was used by the Amiga and the Atari ST developers to implement video access. Both the CPU and the Video controller (Agnus in the Amiga and Shifter/MMU in the Atari ST) are accessing the DRAM by turns both having about 250ns for their transfer. There are very rare cases where the CPU was slightly slowed down by this when e.g. the CPU tried to pause the RAM access for more than 1 cycle and needed to wait one bus cycle for the video to finish it's cycle. But in general both were not interfering during these alternating accesses. Also happening in the video cycle was the refresh DRAM need to keep their memory. This was done in those video cycles that weren't required to actually display data (e.g. in the blank phases  when the beam was returning from right to left or from the screens bottom to the top.

DMA devices (blitter, copper, mass storage, ...) don't have their own cycle. Instead they share the memory cycle with the CPU. This means that the CPU is blocked from accessing RAM in this time. Actually the CPU is blocked from the main system bus, meaning that it cannot access any other peripheral at all during this time. The advantage typially is that those DMA periperhals can do the same job faster than the CPU which e.g. needs to spend time to read instructions telling it what to do while the DMA units have their functionality implemented in hardware so they can use the available memory bandwidth more efficient for their task than the CPU could.

In TG68 based setups the memory access is the part that slows the entire system down to roughly the speed of the original systems. This is unlike the real 68000 CPU which as explained can't go much faster and is not affected by chip/ST RAM speed. The TG68 being clocked at 28 MHz (Amiga) or even 32 MHz (Atari ST) and being more efficient than the real 68000 would be able to perform at rougly ten times the performance it does when being tied to chip/ST RAM.

## Fast RAM / Alternate RAM ##

The so called fast RAM is RAM that is exclusively available to the CPU and is not shared with DMA devices or a video controller. This may actually not be faster than the chip/ST RAM. Amiga SLOW ram is technically fast ram (not available to DMA or video) but runs at the same speed as chip ram. On the atari ST there were even memory expansion card providing alternate ram that is slower than ST RAM. The name "alternate RAM" seems more appropriate here.

But in general having RAM that doesn't have to be shared with other units means that the access can be much faster. Although as already explained, the normal 68000 CPU is barely slowed down by chip RAM/ST ram, so there's not much room for improvement. Things change significantly if a faster CPU (e.g. some accelerator card) is being used. This is usually slowed down significantly when using chip/ST ram and can benefit very much from fast RAM. Technically the TG68 is already such a accelerated CPU which is slowed down by the chip/ST RAM. It would thus benefit greatly from fast RAM.

Fast RAM was much more common on Amigas than alternate RAM was on Ataris. This was mainly due to the fact that the Atari ST was able to address 4MB of ST RAM while the Amiga was limited to 2MB and on some early machines even only 1MB or 512k. Also the Atari can be modified to support up to 14MB ST RAM which later machines like the Falcon even supported out of the box. The Atari/MIST core also supports 14MB of ST RAM.

# SDRAM timing #

The SDRAM interface used here has an external bus cycle rate of 7 MHz (Amiga) or 8 MHz (Atari ST). It can perform one complete SDRAM cycle in this time giving a cycle time of 125ns.

Amiga??

The Atari ST uses two of every four cycles for CPU and Video transfers at the speed of an original Atari ST. One of the remaining two slots is being used for DMA transfers. These could also be moved to the CPU cy cycle freeing up 50% of the total bandwidth for performance or functional improvements. E.g. besides being used to speed up the CPUs memory access this could also be used to increase memory bandwidth for video  and thus allowing for higher resolutions, higher refresh rates or and increased color depth.

Furthermore the SDRAM controller can do 4 cycle bursts within one of these 125ns cycles allowing it to transfer 4\*16 consecutive bits within one cycle. This can be used to prefetch data for future CPU accesses (without delay as the data required by the CPU has already been read) or it can also be used to increase video performance four times.

Overall the re-implementation of the stock Amiga or Atari ST thus only uses 1/8 of the available memory bandwidth.

## SDRAM interfaces ##

The sdram controller as well as the TG68K wrapper implement two completely different interfaces. The "chip" interface being used for chip RAM transfers in the Minimig and ST ram transfers in the MIST. And the "cpu" interface being used to implement fast RAM in the Minimig.

Since both interfaces are used to connect to the very same RAM, it's possible to mix chip and fast RAM features. It's e.g. possible to speed up the CPU interface by using the fast RAM interface and at the same time running video access using chip ram interface.