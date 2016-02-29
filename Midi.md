# Introduction #

The MIST board can be equipped with a physical MidiAddon interface or it can redirect MIDI OUT through its on-board USB device interface.

In both cases the MIDI implementation looks like a genuine Atari ST MIDI port to the software and can be used without any further drivers.

Both interfaces can even be used at the same time (with the USB device interface only supporting MIDI out).

# Real MIDI #

The physical MIDI interface requires a hardware MidiAddon and provides the same physical connectors a real Atari ST has typically on the left side.

The timing implemented with this is very close to the timing of a real Atari ST and the latency of these signals is on-par with that of the original machine.

Unfortunately the timing of the CPU core used on the MIST is not 100% perfect. While this doesn't effect MIDI timing in any way it does prevent many dongles from being implemented in the MISTs FPGA. This has been verified with the Cubase2 dongle.

A video of a synthesizer connected to the MIDI addon board:

<a href='http://www.youtube.com/watch?feature=player_embedded&v=phqs-UzhIL4' target='_blank'><img src='http://img.youtube.com/vi/phqs-UzhIL4/0.jpg' width='425' height=344 /></a>

# Redirected MIDI via USB #

The MIST board has a USB device port (the Micro USB one also used the power the board) which implements a CDC ACM interface. Under windows this shows up as a COM port ([windows driver inf](http://code.google.com/p/mist-board/source/browse/trunk/bin/firmware/mist.inf)).  Under Linux or MacOS X similar device file entries are created under /dev without any special driver being required.

This virtual COM port can be used for various purposes (e.g. debugging or redirecting printer or serial output). This can be selected using the "USB I/O" entry in the System submenu of the OSD. Select "midi" to redirect MIDI output to the CDC ACM interface.

This interface can be used by most standard terminal programs and e.g. using rufus on the ST side and Tera Term on the PC side it's possible to send data from the ST to the PC with the ST using it's MIDI port internally.

## Using Linux as a MIDI synthesizer ##

A Linux PC can be used to playback MIDI audio sent from a MIST board via its USB device port. This can be used with most MIDI enabled software like certain games (e.g. ECO) or sequencing software (like cubase lite).

Various MIDI software synthesizers exist for Linux. One popular example is the [timidity](http://timidity.sourceforge.net/) program.

Unfortunately these programs are not designed to work on raw byte streams as MIDI input. This is due to the fact that PCs are inherently bad at sustaining a precise timing without help. Thus typical PC based setups include MIDI interfaces that take care of certain timing constraints. The simple CDC ACM interface does not support this.

To allow programs like [timidity](http://timidity.sourceforge.net/) to operate on ordinary COM ports a additional software like [ttymidi](http://www.varal.org/ttymidi/) is required which feeds the raw byte stream coming from the CDC ACM interface into the MIDI channels of a typical Linux audio system.

A typical setup for this starts with the redirection of the raw bytes stream into a MIDI source:

`ttymidi -s /dev/ttyACM0`

Then synthesizer is need as a MIDI sink. Timidity can be used for this:

`timidity -iA`

as well as fluidsynth:

`fluidsynth -a pulseaudio -m alsa_seq /usr/share/sounds/sf2/FluidR3_GM.sf2`

Then the MIDI sources and sinks need to be logically wired up. The command

`aconnect -iol`

will list all MIDI channels and their connections like this:

```
client 0: 'System' [type=kernel]
    0 'Timer           '
    1 'Announce        '
        Connecting To: 131:0
client 14: 'Midi Through' [type=kernel]
    0 'Midi Through Port-0'
client 128: 'TiMidity' [type=user]
    0 'TiMidity port 0 '
    1 'TiMidity port 1 '
    2 'TiMidity port 2 '
    3 'TiMidity port 3 '
client 129: 'ttymidi' [type=user]
    0 'MIDI out        '
        Connecting To: 130:0
    1 'MIDI in         '
client 130: 'FLUID Synth (4184)' [type=user]
    0 'Synth input port (4184:0)' 
        Connected From: 129:0
```

In this case the MIDI out port of client 129 (the ttymidi program) is already connected to the input of client 130 (the FLUID Synth synthesizer).

If these were not already connected one could connect them using the command line:

`aconnect 129:0 130:0`

or alternally a graphical interface like qjackontrol:

![http://mist-board.googlecode.com/svn/wiki/jack.png](http://mist-board.googlecode.com/svn/wiki/jack.png)

Afterwards it's possible to play back MIDI music from the MIST Atari ST through the Linux PC acting as a synthesizer.

<a href='http://www.youtube.com/watch?feature=player_embedded&v=Jrk_ftOi7z0' target='_blank'><img src='http://img.youtube.com/vi/Jrk_ftOi7z0/0.jpg' width='425' height=344 /></a>

Tested with:
  * Cubase Lite
  * Cubase 3 (cracked version)
  * Eco
  * Karate Kid II
  * Pirates