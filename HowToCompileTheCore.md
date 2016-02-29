Up to now the cores have only be compiled under Linux. This page thus only describes the necessary Linux setup. The same instructions probably also work under Windows since the same tools being used are available for Windows as well.

# Prerequisites #

The cores are written in a combination of Verilog and VHDL. A synthesis tool is required to compile this into a binary FPGA configguration (/sof/rbf). Since the FPGA on the MIST is a member of the Altera Cyclone III family, the Altera Quartus II suite is required. The free Web Edition is sufficient. See e.g. http://www.altera.com/products/software/quartus-ii/web-edition/qts-we-index.html

# Compiling #

The core source code is available from the [MIST svn repository](http://code.google.com/p/mist-board/source/checkout).

To load the project simple open the project file (**.qpf, e.g. core/mist/mist.qpf) in quartus. On the left side under "Flow" select "Compilation". and double click "Compile Design". Depending in the speed of your computer this may take from some minutes up to an hour.**

# Installation #

Two files are generated in the out directory during compilation:

  * **mist.sof** is a "SRAM Object File". This can be uploaded to the FPGA using a Byte Blaster JTAG cable or similar
  * **mist.rbf** is a raw binary file. This can be upoaded to the FPGA from SD card using the MISTs on-board IO controller

To allow for auto configuraton the raw binary file of the default core needs to be renamed to core.rbf and placed in the root directory of the SD card. The IO controller will find it at system initialization and upload it to the FPGA.