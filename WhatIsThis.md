The following explanations are not 100% exact. Instead they are simplifying certain aspects to make the basic topics understandable.

# What is the MIST board? #

The MIST board is built around a so-called FPGA chip with "cores" written in HDL. So the question what the MIST board is leads to the question what an FPGA is.

## What is FPGA and this HDL it uses? ##

They are essentially a custom microchip used to build a System-On-a-Chip on demand.

Nornally creating microchips requires a large factory and schematics of the hardware. This led to the development languages to describe schematics (HDL) and tools to prototype chip designs before they are "baked" onto silicon (at which point they can no longer be changed). These tools are programmable chips such as FPGAs.

Recently the price of FPGAs has gone down significantly, at which point it is cheaper to sell devices with an FPGA inside rather than producing dedicated chips. Each units is more expensive but they cost less to build and are upgradeable. Also the availability of free HDL development tools, and HDL code for most cores being open source  means that each device can be its own development kit -allowing anybody to tinker.

Other examples of devices with FPGAs are the SD2SNES cartridge, the EverDrive N8 cartridge, and the XRGB-Mini upscaler (not all use open source cores).




## So what exactly is an FPGA? (detailed) ##

An FPGA is an intengrated semiconductor. It consists if millions (sometimes even billions) of small logic elements just like any microprocessor, memory chip or IO chip in a standard PC. In a microprocessor the logic elements are wired in a way that they finally show the behaviour that the microprocessors vendor intended. In a memory chip the logic elements are wired in a way that data can be stored inside the chip. This is achieved by the manufacuturers by selecting exactly the logic elements the intended chip type requires and by wiring them up inside the chip to perform exactly the function the manufacturer wants them to do.

FPGAs are a little different. They also consist of a huge number of logic elements. But these aren't the logic elements required for a particular type of chip. Instead the logic elements are some kind of "useful selection". What exactly that means differs a little bit from FPGA to FPGA. Furthermore, these logic elements are not directly wired together. Instead all possible connections are there, but each wire includes a little switch allowing this connection to be made or not. Finally the FPGA contains some memory. Each of the aforementioned switches is cnnected to one bit of this memory.

All those millions of switches can thus be influenced by loading data into the FPGAs internal memory. This data determines which switches are closed and which are left open. This in turn causes all the logic elements to be wired up in a certain way.

## Configuring an FPGA ##

A hardware developer requiring a certain logic inside the FPGA creates a textual representation of this logic in a so-called hardware description language (HDL). Popular examples for such languages are Verilog and VHDL. This description is then fed into a so-called synthesis tool which in turn creates the aforementioned data from it. The file containing the data is usually named the "FPGA configuration file" or often just "core".

An FPGA can be reconfigured at any time just by downloading a new core into it. It can completely change its function by doing that. The FPGA can be configured to implements any type of microprocessor, memory or IO chip. It is possible to build entire computers out of nothing but FPGAs.

## The disadvantages of FPGAs ##

Why aren't all computers just built of FPGAs? The space required inside an FPGA to implement a certain logic including the logic elements, the switches and the memory is much bigger than the space required inside an application specific chip like a microrocessor or memory chip with hard-wired login. These just consist of the logic elements and their connections and no additional switches or memory cells are required. Also the selection of logic elements inside an FPGA may not be perfect for a certain application and a standard microprocessor or memory chip instead contains exactly those logic elements that are required by the desired function. Also the more complex internals of an FPGA limit the maximum clock speed below those speeds that can be achieved by application specific chips. And finally the lower sales volume of FPGAs makes them much more expensive than those chips that are sold computers, phones and other gadgets. The most expensive chips available are big FPGAs and it's not difficult to pay more than $20,000.- for a single chip.

This is the reason why the MIST board contains a SDR-SDRAM memory chip connected to the FPGA. In theory it would be possible to just use one single big FPGA instead of this FPGA + memory combo. But the intended use of the MIST board will sure include memory and as explained it's much more expensive to implement memory inside an FPGA than using standard memory chips. Thus the current solution is much cheaper and doesn't limit the use-case much. The MIST board also contains a microcontroller mainly to initialize the FPGA at startup. Once the FPGA is up and running the microcontroller acts as a support chip and allows to implement things in software that are difficult to implement inside an FPGA. But the microcontroller is not a must and similar setups exist that do not use one.

## FPGAs for retro computing ##

But even the smaller and cheaper FPGAs today are big enough to contain the entire logic of a 20 years old computer. Today most of the chips used in those old machines are not produced anymore. There are two ways to run the old code on modern hardware: Using software that runs on todays computers and makes them behave like the old machines. This is called emulation. Or re-implement the entire logic of those old machines inside todays FPGAs. Some people also call this "emulation". But to differentiate this from the use od FPGA terms like "re-implementation" are preferred.

## What's better? Emulation or Re-implementation? ##

That's the main question. Unfortunately there is no simple answer.

In theory a FPGA can be used to built a perfect replica of those old machines. This can be done much more accurate than any software emulator on a PC will ever be since the emulation on the PC is bound to certain timing limitations imposed by the PC hardware and the operating system running on it. The emulator will always be affected by other things happening on the PC in the background. These effects are typically less on faster PCs. In order to emulate a computer of the Amiga or Atari ST line in a satisfying way a pretty fast PC is required.

FPGAs don't have these problems. Any digital function can be implemented on the FPGA at a very high precision. Furthermore FPGA can be used to implement old hardware interfaces to connect to old legacy hardware like joysticks or floppy drives. This requires additional hardware on a PC and often just isn't feasible.

In reality a very important factor plays the most significant role: The user base of emulators is huge. Most people have a PC and can run an emulator at no additional cost. Thus emulators have many users and also many developers working on them. And even though it's much more difficult to implement certain things in an emulator than in an FPGA the bigger developer and user base usually gives great and near-perfect results.

Using an FPGA based system on the other hand requires the user to purchase a certain hardware. The user and developer base is thus much smaller. On the other hand it's easier to recreate an old computers hardware with hardware FPGAs than with software emulators. Thus the compatibility achieved by FPGA based setups is pretty good.

## Why should i get an FPGA based setup then? ##

The FPGA based setups are usually tiny power efficient devices. Instead a PC required for a decent emulation of a Commodore Amiga or Atari ST is a big power hungry box.

An emulator is fine if you already have a PC and don't mind using it for your retro gaming needs. If you want to have a tiny box that you can just carry around and easily connect it to a TV for a quick session of Goldrunner, then get an FPGA based setup.

Also the PC needs to be booted into its own operating system first. The FPGA instead brings back the old "instant on" experience of those early machines and can boot into a game in a few seconds.

Gaming targetted FPGA boards like the MIST usually come with connectors for the old joysticks. PCs usually use modern game pads. But many people prefer using the original joysticks with the old games.

FPGAs are fancy. They are unlike any technology used in todays PCs.