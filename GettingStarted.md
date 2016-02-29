# Introduction #

This page will tell you how to do the initial setup of your MiST FPGA board. It explains how to setup a SD card to boot into an Atari ST or Amiga setup for the first time. Afterwards you can continue [updating your board to the latest firmware and core](HowToInstallTheFirmware.md) or start using other cores.

# What you need #

The MIST board was designed for simplicity. Getting it up and running is thus pretty easy. First of all you'll need a few additional things.

Mandatory:

  * The MIST board
  * A USB keyboard
  * A micro USB phone charger for power supply (a micro USB cable connected to a USB hub or PC also works)
  * An SD card (1GB recommended)
  * A VGA screen

Optional:

  * A USB mouse
  * A classic Atari style joystick or USB joystick or gamepad
  * A set of PC speakers

For potential compatibility issues see the [peripherals wiki page](Peripherals.md).

# Preparations #

First you have to prepare the SD card. Place the following files in the root directory of a standard FAT formatted SD card. You can choose to do a Atari ST or Amiga setup. It's recommanded to do the initial test with the Atari ST setup since all the required files are available for download below. The Amiga setup requires copies of copyrighted files.

## Atari ST ##

  * [The early Atari ST FPGA core (core.rbf) for boards purchased before May 2015](http://code.google.com/p/mist-board/source/browse/trunk/bin/cores/mist/core.rbf) or
  * [The Atari ST FPGA core for boards purchased May 2015 or later](http://mist-board.googlecode.com/svn/trunk/bin/cores/mist/core_150413_r1017.rbf)
  * [A (Emu-)TOS rom image (tos.img)](http://code.google.com/p/mist-board/source/browse/trunk/bin/cores/mist/tos.img)
  * [A default floppy disk image for drive A:](http://mist-board.googlecode.com/svn/wiki/disk_a.st)

## The pre-installed firmware version ##

The MIST board uses a permanently installed firmware in on-board flash as well as the called FPGA core file on SD card. Since both parts evolve over the time they need to be from roughly the same time. If the versions of the firmware and the core differ too much then the core may not boot (but no harm will/can be done).

Up to now two firmware versions have been installed in MIST boards before shipment.

If you purchased your board before May 2015 or if you got an older board from the third party you need to use the [early core](http://code.google.com/p/mist-board/source/browse/trunk/bin/cores/mist/core.rbf) mentioned for your first tests.


Since May 2015 Lotharek started to flash boards with a firmware from April 2015. That version requires a [more recent core](http://mist-board.googlecode.com/svn/trunk/bin/cores/mist/core_150413_r1017.rbf). You need to rename this core to `core.rbf` before placing it on the SD card.

It doesn't cause any harm if you confuse the core versions. They'll just not boot and you'll probably get a messed video output. Replace the wrong core with the correct version and you'll be fine again. Technically all boards are the same and once you are successful with your initial tests you may update any board to the latest firmware and latest core. You can always check the firmware version by opening the OSD (by pressing F12) and then selecting "Firmware & Core". This will show the installed firmware version under "ARM s/w ver." as depiced below.

<img src='http://mist-board.googlecode.com/svn/wiki/img_docs/osd_fw_ver.jpg' title='OSD Firmware version' />

The Atari [system.fnt](http://code.google.com/p/mist-board/source/browse/trunk/bin/cores/mist/system.fnt) can optionally be placed on the SD card and will then be used for initial boot messages (otherwise the same font as for the OSD is being used).

You may also replace the EmuTOS image with a real TOS image. Any language of version TOS 1.00, TOS 1.02, TOS 1.04 or TOS 2.06 should work. Please be aware that copyright still applies to these.

Booting may be very slow with no floppy disk enabled/inserted. This you should put http://mist-board.googlecode.com/svn/wiki/disk_a.st also onto the SD card. TOS will automatically pick this up and boot times will be much faster.

## Amiga/Minimig ##

  * [The Minimig FPGA core (core.rbf)](http://code.google.com/p/mist-board/source/browse/trunk/bin/cores/minimig/core.rbf)
  * A kickstart ROM image named kick.rom (Kickstart 1.2, 1.3 and 3.1 have been tested).

# Setup #

Insert the SD card into the MIST. Connect VGA screen and USB keyboard. Finally connect the phone charger.

# Go! #

Switch the board on. It should boot into the Atari ST desktop or into the kickstart splash screen.

# Troubleshooting #

The board doesn't boot? Please read about [Troubleshooting](Troubleshooting.md)

# Upgrade to the latest version #

Now that you veryfied that the board is working and the Atari ST boots it's time to upgrade your board to the latest firmware and core. The latest firmware is the one with the highest revision number found at

http://code.google.com/p/mist-board/source/browse/trunk/bin/firmware

To update the firmware rename the latest version to `firmware.upg` and put it onto your sd card. Boot into your Atari ST or Amiga setup and go to the `Firmware & Core` submenu, select `Update` and follow the instructions. For details can be found at HowToInstallTheFirmware.

To use e.g. the latest Atari ST core is the one with the highest revision number found at

http://code.google.com/p/mist-board/source/browse/trunk/bin/cores/mist.

To replace the core simply rename the new version to `core.rbf` and replace the old version already on your SD card.

Some information about the updates and changes these bring can be found at

[the firmware and core changelog thread](http://www.atari-forum.com/viewtopic.php?f=101&t=26513).

# Things to do next #

Once the board works there are many things to explore:

  * Connect a USB keyboard and press F12 (or the menu key) to explore the on screen display (OSD)
  * Press the "NumLock" key to cycle through mouse and joystick emulation (use the cursor keys for this)
  * Connect a USB mouse
  * Connect speakers
  * Add floppy disk images (.st format for the Atari ST and .adf files for the Amiga) to the SD card and select them via the OSD
  * Visit the [MIST section in the Atari Forum](http://www.atari-forum.com/viewforum.php?f=101), or
  * visit the [MIST thread in the Minimig Forum](http://www.minimig.net/viewtopic.php?f=3&t=546)