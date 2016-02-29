# What peripherals are supported? #

The MIST currently supports:

  * Power supply
  * USB keyboards
  * USB mice
  * Classic C64 style joysticks
  * SD cards
  * VGA screens
  * Speakers
  * USB joysticks/gamepads

Currently unsupported are:

  * Original Atari ST and Amiga mice
  * USB storage devices (thumb drives)
  * USB bluetooth dongles

Support for these is a matter of firmware support and they may be supported one day.

# Power supply #

The board is powered via micro USB. The board draws ca. 300mA/5V. Additional power is required for attached perpherals. A total of 400mA is sufficient. The board can thus easily be powered from any PC USB port (providing 500mA) or standard USB phone charger (providing 1200mA).

# USB keyboards #

A USB keyboard to be used with the MIST needs to support the so-called "USB HID Boot mode". This is a simplified mode of communication with the keyboard mainly intended to be used when controlling the BIOS of a PC via USB keyboard. All USB keyboards tested so far support this and work properly on the MIST. This includes the keyboard part of all wireless keyboard mouse combos tested so far (inkl. the Rii and the K400).

# USB mice #

USB mice to be used with the MIST also need to support the "USB HID Boot mode". Most simple mice tested so far work fine. Very few mice have been reported not to work. Also the mouse part in some wireless keyboard mouse combos doesn't work properly. If in doubt use a simple wheel mouse of a well-known brand.

The symptoms of mice not working typically is a mouse cursor "jumping around" when moving the mouse. The reason for this behaviour is that the mouse did not properly switch into Boot mode and sends movement information in a format other than the one used in Boot mode.

Only one mouse is presented to the system. If more than one mouse is connected they all drive the system mouse. From the embedded systems view (Atari ST / Amiga) only one mouse is connected. E.g. it's currently not possible to play two player dual-mouse games like Lemmings for the Amiga this way.

# Wireless USB keyboard/mouse combos #

For unknwn reason small wireless USB keyboard mouse combo devices have a high probability to run into the boot mode problem. A device know to work well is the [Logitech K400](http://www.logitech.com/de-de/product/wireless-touch-keyboard-k400-business?crid=656). The touchpad of a [Rii mini wireless](http://www.riitek.com/en/product-detail-424.html) on the other hand does not work.

It is currently unclear whether this is a bug in those devices or whether this is a bug in the MIST firmware.

These wireless combo devices are known to work:

| **Brand** | **Description** | **Image** |
|:----------|:----------------|:----------|
| Logitech  | K400 and K400r  | [Image](http://imgur.com/HqIGIjD.jpg) |
| Rapoo     | E2000P and 8000 | [image](http://imgur.com/r7sbwDA.jpg)|


# Bluetooth USB and mice #

Bluetooth devies are currently not supported.

# Classic C64 style joysticks #

Two classic C64/Atari/Amiga style joysticks with standard DB9 connector are fully supported. Two fire buttons are also supported on systems that support this. The joystick ports are provided with up to 100mA each so that auto fire functions and LEDs are supposed to work as expected.

Not supported on these ports are mice (Atari ST nor Amiga mice), light guns or analog paddles.

# USB joysticks and gamepads #

USB joysticks and gamepads are not trivial to connect. The status reports they send vary from device to device. The MIST firmware supports many of them, but not all. Chances are better with cheap and/or simple devices.

### USB controllers known to work ###
This list is not exhaustive, but should give you an idea of which controllers work:

| **Brand** | **Description** | **Image** |
|:----------|:----------------|:----------|
| iBuffalo  | `SNES` classic controller | [image](http://i.imgur.com/CUBgkJK.jpg)|
| Buffalo   | 'Famicom' classic controller | [image](http://i.imgur.com/BOoicBv.jpg) |
| Retrolink | `NES` gamepad replica (rectangular case) |           |
| Retrolink | `GameCube` gamepad replica|           |
| Retrolink | `N64` gamepad replica | [image](http://i.imgur.com/hYIIym0.jpg)   |
| Retrolink | `Atari2600` joystick replica |           |
| Speedlink | Competition Pro USB |           |
| (unbranded) | Cheap SNES gamepad | [image ](http://i.imgur.com/YvuZpqk.jpg)|

The N64 gamepad replica can act as an analog joystick.


## USB vs. atari style joysticks using more than two joysticks ##

A real Atari ST has two Atari Style joystick ports. Port 0 is used for the mouse and port 1 is used for the joystick. If two joysticks are required the user has to unplug the mouse and use a second joystick in port 0.

The MIST board also has two such connectors. We call them the "physical joystick ports". Since the mouse is connected via USB to the MIST both physical ports can be used for classic atari style joysticks.

If you connect a classic atari joystick to physical port 1 you can play single player games. If you instead connect a USB joystick you can also play single player games.

If you connect two USB joysticks or one USB joystick and one classic Atari joystick to physical port 1 you can play two player games. The USB joystick will then become the "primary" joystick.

If you plug more than two joysticks the additional ones behave like they were connected via a Gauntlet II compatible multi player adaptor.

# SD cards #

Standard form factor classic SD cards (<= 2GB) as well as SDHC cards (> 2GB) are supported. The use of a 1GB card of a well-known brand is recommended.

The card has to be FAT formatted. If in doubt use a windows PC or even better a device like a digital camera to format the card.

# VGA screens #

The video timing on the VGA output depends on the core being used (Atari ST / Amiga / whatever) and the compatibility varies from core to core and from screen to screen. Some of the classic TV video modes (e.g. the Atari ST color video modes) use a screen refresh rate that's outside the VGA standard although many screens can display it, anyway.

For further information read e.g. the video section in HowTheAtariStCoreWorks.

# Speakers #

Any type of active PC speakers with built-in amplifier should be fine.