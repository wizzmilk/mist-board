# Rom Management #


# Do you know this ? #
You found a nice Collection of old Games on the Web, put it on your SD card with the needed core and start it on your MIST. Thousands of Roms/Disks/whatsoever are now floating on your SD. Plenty of Duplicates, Bad Dumps are also now on your SD Card too which are not needed.
Filenames are incredible long because each file can also contain Year, Cracker Group, Developer of the Game, Trainer Info, Doc, Disk number and so on. While this all is needed for the serious Rom Collector, for your MIST FPGA you don't need it except the Disk Number. It affects the readability on the MIST OSD and all the Dupes and Bad Dumps just waste your Space on the SD Card.

# Cleaning the Mess #

To Clean this mess we use a so called Rom Manager and a **Dat** for this. it's purpose is to compare each Rom/Disk/whatsoever against a Database and sort out all those unneeded stuff.
In this case we use RomVault because its very easy to use and we can build a Directory Structure for the SD-Card.

# What does RomVault do and what's a Dat ? #

A Dat is a simple Text file with the Name of the Game and in my case 2 Checksums. A CRC and MD5 Checksum. In other Words, it's a Database. RomVault compares each Rom/Disk/.. against the Database. It does this by comparing the Checksum of each found file against the Checksum of the Database. If both are the same, the Rom/Disk/.. is a “good” one and all other ones who are not found from RomVault are Dupes and Bad Dumps.

# Isn't that a lot of Work ? #

To be Honest...No. Once the Preparations are done, you only click 4 Icons. Update Dats, Scan Roms, Find Fixes and Fix Roms. After that we need to mass unzip all files which will be done by using ExtractNow. That's all to build a clean Set of Games with nearly No Dupes and No Bad Dumps.

Dats are already avaiable for:

  * Amiga
  * Amstrad
  * Astrocade
  * Atari 2600
  * Atari 5200
  * C64
  * Chip8/SuperChip
  * Colecovision
  * NES
  * PCEngine
  * Sega Master System
  * Videopac / Magnavox

A step by Step Instruction PDF and the Dats can be found here: http://mist-board.googlecode.com/svn/trunk/rom_manager