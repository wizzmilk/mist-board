# Overview #

This page has operating instructions for the PC Engine core.

# Links #

  * [Core binary releases](http://retroramblings.net/?page_id=965)


# Installation #

At the moment, this core only supports reading files from the SD root.
It is thus recommended to use it on its own SD card to make it easier to select rom files.

Copy the `*`.rbf file at the root of the SD card.
You can rename the file to core.rbf if you want the MiST to load it automatically at startup.

Also copy the config file provided in the binary distribution. Select the option that fits your setup (e.g. NTSC if using an LCD display that expects 60hz). The config can be changed later, but the file must already exist to save it.

Copy PCE roms in the same root folder.


---


# OSD Menu #

Press F12 to enter the OSD menu. Move with arrow keys and select with Enter:

<img src='http://mist-board.googlecode.com/svn/wiki/img_docs/pce_osd1.jpg' title='OSD Menu - Page 1' width='320px' />

  * **Reset**: restart the PC Engine with current rom
  * **Save Settings**: save current core settings for next session
  * **Video Mode**: Switch between NTSC, PAL, and TV (15khz) mode.
  * **Scanlines**: activate scanlines - best used with NTSC or PAL (won't work well with TV mode)
  * **Machine mode**: PC Engine or TurboGrafx
  * **Load Rom**: select a ROM to load

In the rom selection screen, you can select a rom with Enter,
but you can also load as PC Engine or TurboGrafx by pressing P or T.
(try the other mode if loading the rom results in a black screen)


## Notes ##

  * PAL mode is 720x576p @50hz
  * NTSC mode is 720x480p @60hz
  * TV (15khz) mode is 720x241p @60hz


# Compatibility #

[This spreadsheet](https://docs.google.com/spreadsheets/d/1X2nJAMh3xoumbsaXSxzLa0r-JfySNVOsEy2Pw42cOls) has a (incomplete) list of games that were tried with the core.

Most games seem to work with the PC Engine mode; so it is recommended to use this mode by default. When some show a blank screen (or freezes after a few frames), you can try loading with the other mode.



---



# Gameplay videos #

<a href='http://www.youtube.com/watch?feature=player_embedded&v=eqkAILkPe5I' target='_blank'><img src='http://img.youtube.com/vi/eqkAILkPe5I/0.jpg' width='425' height=344 /></a>

<a href='http://www.youtube.com/watch?feature=player_embedded&v=4l58HPSzfjQ' target='_blank'><img src='http://img.youtube.com/vi/4l58HPSzfjQ/0.jpg' width='425' height=344 /></a>

<a href='http://www.youtube.com/watch?feature=player_embedded&v=CzeHW-gyMSI' target='_blank'><img src='http://img.youtube.com/vi/CzeHW-gyMSI/0.jpg' width='425' height=344 /></a>

<a href='http://www.youtube.com/watch?feature=player_embedded&v=OvreesBg8AE' target='_blank'><img src='http://img.youtube.com/vi/OvreesBg8AE/0.jpg' width='425' height=344 /></a>


---


