# LEDs #

The three LEDs are the most fundamental indicators on the board and may help you find the source of problems. Their meaning is:

  * Green power LED: This is connected to the 3.3V power rail. It must light up as soon as the board is connected to a power supply. Make sure you have a stable 5V power supply connected to either the DC jack (prototype boards) or the micro usb connector (series board). You need at least 200mA for the board itself to boot, during operation at least 500mA is mandatory, 1A is recommanded if you want to use USB peripherals.
  * Yellow core LED: The yellow LED is connected to the FPGA and its use is core specific. The Amiga core e.g. uses it as the software controlled Amiga Power LED and the Atari core uses it as a floppy LED.
  * Red IO controller LED: The red LED is connected to the IO controller. It's usually used to indicate SD card IO when the system is running properly. But during the early boot phase it is also used to display fatal errors using blink codes.

## Red LED blink codes ##

The blink codes consist of a sequence of on-off events with a 1 second pause in between. The number of times the LED blinks between two pauses has a meaning:

  * Blinking once: The SD card could not be initialized. Please insert an SD card.
  * Blinking twice: No usable file system was found on the SD card. Please make sure the card is properly formatted (e.g. digital cameras usually can format SD cards).
  * Blinking three times: FPGA doesn't respond to configuration request. There's a problem with the FPGA or the connection between FPGA and IO controller
  * Blinking four times: No valid FPGA file found on card. Make sure there's a valid core.rbf on the card
  * Blinking five times: The FPGA did not respond properly to the initialization attempt. Make sure there's a valid core.rbf on the card

# Troubleshooting the IO controller #

If the red RED led blinks, then the IO controller is flashed with at least some version of the firmware and is working. The RS232 may give further information on the problem.

## RS232 ##

The MIST boards IO controller has a serial interface used mainly for development and debugging.

In the prototypes this is wired to a real rs232 interface (some jumpers have to be set correctly) and the IO controller uses this to output extensive debug information incl. Firmware version and information about the SD card, the data being uploaded to the FPGA. Just attach a RS232 cable or USB-to-RS232 converter to this port is sufficient.

The series boards don't have the full rs232. Instead the serial port of the ARM io controller is present on connector SV3, a four pin header close to the ARM IO controller next to the SD card slot. If this port is to be used for debugging, development or trouble shooting, then a rs232 converter like [this one](http://www.emartee.com/product/41777/) needs to be used. [Similar devices](http://www.elecfreaks.com/store/usb-to-rs232-converter-p-228.html) exist for use with USB.

The parameters used on the RS232 port are 115200 bit/s 8n1.

## USB ##

The MIST board is powered via USB. The USB device port used for this is in fact a fully functional USB link. Under Linux and MacOS it is detected by the host out of the box. Under Windows the following file can be used to install the appropriate driver: http://mist-board.googlecode.com/svn/trunk/bin/firmware/mist.inf

This installs a COM: port under Windows or a device file like /dev/ttyACM0 under Linux. Both can be used by ordinary terminal programs like hyperterm under Windows or minicom under Linux. It doesn't matter which communication parameters (e.g. baud rate) is setup as these are meaningless for USB. Whenever you reboot the MIST, the connection is re-established and you might have to restart the terminal program on your PC.

Boot into a Atari ST core and open the On Screen Display (OSD). Go to the System submenu and select "debug" at the "USB I/O" entry. From now on you the debug output that is usually sent via rs232 (see above) is also forwarded to the USB interface.

To debug a Amiga/Minimig core you first have to boot into a Atari ST core. Make sure there's also a minimig core on the SD card named e.g. minimig.rbf. When in Atari ST mode open the OSD again, go into the "Firmware & Core" submenu and select "Change FPGA core". Select the minimig core and you should see IO controllers debug output.

# VGA output #

Some little debug output is also done via the VGA output. But this is rather limited as most parts on the board need to work in order to display something on the VGA output.

# FPGA debugging #

It is even possible to do live debegging of FPGA internals. This required in-depth know-how of the FPGA, the ability to synthesize special debug versions of the core and a USB-Blaster cable to connect to the core running inside the FPGA.