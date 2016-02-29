# Overview #

The USB2Serial debugging board allows you to watch the debug output of the ARM IO Controller. This is useful mainly for development and debugging, but may also help to trace problems.

![http://mist-board.googlecode.com/svn/wiki/serial_connected.jpg](http://mist-board.googlecode.com/svn/wiki/serial_connected.jpg)

The USB2Serial board connected to a V1.2 pre-production board.

# Schematic #

The board connects via SV3 to the ARM IO Controller debug connector of the MIST board V1.2 or V1.3.

![http://mist-board.googlecode.com/svn/wiki/usb2serial_schematic.png](http://mist-board.googlecode.com/svn/wiki/usb2serial_schematic.png)

[Schematic(PDF)](http://mist-board.googlecode.com/svn/wiki/usb2serial_schematic.pdf)

# Printed Circuit Board #

The board plugs directly into the MIST V1.2 or V1.3 (the first series board) board. Previous development boards came with rs232 transceivers and don't need this addon.

![http://mist-board.googlecode.com/svn/wiki/usb2serial_pcb.png](http://mist-board.googlecode.com/svn/wiki/usb2serial_pcb.png)

[Board(PDF)](http://mist-board.googlecode.com/svn/wiki/usb2serial_pcb.pdf)

# Debugging #

The communication parameters are 115200 bit/s 8N1.

![http://mist-board.googlecode.com/svn/wiki/serial_pcb.jpg](http://mist-board.googlecode.com/svn/wiki/serial_pcb.jpg)

An example debug output will look like this:

```
Minimig by Dennis van Weeren
ARM Controller by Jakub Bednarski

Version ATH130522

SDHC card detected
spiclk: 24 MHz
usb_init
max3421e_init()
Chip revision: 13
busprobe()
usb_reset_state
partition type: 0x72 (FAT16)
fat_size: 240
fat_number: 2
fat_start: 263
root_directory_start: 743
dir_entries: 512
data_start: 775
cluster_size: 32
cluster_mask: FFFFFFE0
file "CORE    RBF" found
FPGA bitstream file opened, file size = 367568
[***********************]
FPGA bitstream loaded
FPGA configured in 857 ms
Identified MiST core
Running mist setup
file "SYSTEM  FNT" not found
Uploading TOS ...
file "TOS     IMG" found
TOS.IMG:
  size = 196607
WARNING: Unexpected TOS size!
  blocks = 383
  address = $00fc0000
Uploading: [................................................]
TOS.IMG uploaded in 231 ms (851 kB/s / 6808 kBit/s)
Erasing cart memory
file "DISK_A  ST " found
A: eject
A: insert DISK_A  ST 
A: detected 2 sides with 9 sectors per track
file "DISK_B  ST " not found
file "HARDDISKHD " not found
=> CONDETIRQ
busprobe()
=> BUSEVENTIRQ
usb_configure(parent=0 port=0 lowspeed=0)
using free entry at 0
Setting addr 1
usb_set_addr(new=1)
usb_ctrl_req(addr=0, len=0, ptr=0x0)
trying to init class 0
usb_hub_init()
usb_ctrl_req(addr=1, len=8, ptr=0x20ff28)
usb_ctrl_req(addr=1, len=18, ptr=0x20ff28)
usb_ctrl_req(addr=1, len=8, ptr=0x20ff28)
usb_ctrl_req(addr=1, len=9, ptr=0x20ff28)
usb_ctrl_req(addr=1, len=0, ptr=0x0)
usb_ctrl_req(addr=1, len=0, ptr=0x0)
usb_ctrl_req(addr=1, len=0, ptr=0x0)
usb_ctrl_req(addr=1, len=0, ptr=0x0)
usb_ctrl_req(addr=1, len=0, ptr=0x0)
 -> accepted :-)
```