# What is a Byte Blaster Cable? #

The USB Byte Blaster Cable is a cable that connects a host PC via USB
directly to the MISTs FPGA using a so-called JTAG connection. It can
be used to download FPGA configurations directly from the PC to the
FPGA without using the SD card. Furthermore it allows direct
monitoring and debugging of signals inside the FPGA. Cheap Byte
Blaster Cables can be found on EBay.

![http://mist-board.googlecode.com/svn/wiki/adapters.jpg](http://mist-board.googlecode.com/svn/wiki/adapters.jpg)

# Adding the connector #

The USB Byte Blaster cable comes with a 10 pin flat ribbon cable. The
matching plug is not present on the standard MIST boards but can
easily be added. The required connector is a standard 10 pin 0.1"
straight box header which needs to be soldered to SV1 at the left PCB
side close to the VGA connector. Please be careful with the connectors
orientation. Mounting it in the wrong direction my damage the MIST
and/or the USB Blaster.

# The special meaning of DIP switch 1 #

Usually the IO controller configures the FPGA. In that case the IO
controller of course knows that the FPGA has been configured and react
accordingly.

When using a Byte Blaster cable the IO controller is not involved in
the download process and thus cannot know that the FPGA has freshly
been configured. It thus will not know that i needs to update its
internal infirmations with respect to the new FPGA configuration. The
IO controller needs to be restarted using the push button next to the
SD card slot to re-establish its connection to the FPGA.
Unfortunately upon reset the IO controller will itself upload a new
configuration to the FPGA overwriting anything that might have been
downloaded via JTAG before.

The solution to this is DIP swtch 1 on the MIST board. In it's default
state it is in the OFF position. This is the setting it should be in
normal operation.

When being switched on the DIP switch 1 causes several things:

  * The IO controller does not attempt to reconfigure the FPGA on its own reboot
  * The IO controller tries to monitor the FPGA for JTAG uploads and tries to reset itself whenever it detects that the FPGA config has changed
  * The IO controller increases some debug output and e.g. reports SD card accesses

With the DIP switch 1 in the ON position using a Byte Blaster cable
becomes very easy.