# Intro #

The MIST is powered via micro USB. This allowed to power the board from small, cheap and esily available chargers. Furthermore there's no risk of using the wrong voltage or wrong polarity. You MIST is safe when powering it over micro USB.

Another common method to power devices of this size is the standard 5.5mm DC jack.

The MIST board can be modified to use a standard DC jack instead of micro USB.

# There can only be one #

Only one power supply option can be used at a time. Just like the principle of the highlander "There can only be one". You cannot power your board from micro USB anymore if you decide to convert your board to be powered from a DC jack. This is to avoid shortening the two power supplies when connecting the DC jack and the micro USB at the same time.

# More caveats #

  * Be sure the DC power supply provides 5V. Everything else may damage the board
  * Be sure the positive terminal is fed into the central tip. Wrong polarity will damage the board
  * Use a power supply that delivers at least 500mA, better 1A
  * Soldering in your board voids the warranty
  * You may also need to drill a hole in the case

If you still want to modify your board, here's how ...

# How to do it #

You need a soldering iron, some basic SMD soldering skills and a matching DC plug like [this one](http://www.reichelt.de/Hohlstecker/HEBW-21/3/index.html?;ACTION=3;LA=446;ARTICLE=35273;GROUPID=3258;artnr=HEBW+21).

First you need to move the 0-ohms resistor from position [R2](https://code.google.com/p/mist-board/source/detail?r=2) to position [R1](https://code.google.com/p/mist-board/source/detail?r=1). This cuts the power connection to the micro USB port and connects the DC jack.

![http://mist-board.googlecode.com/svn/wiki/pwr_dcjack_1.jpg](http://mist-board.googlecode.com/svn/wiki/pwr_dcjack_1.jpg)

Then add the connector.

![http://mist-board.googlecode.com/svn/wiki/pwr_dcjack_2.jpg](http://mist-board.googlecode.com/svn/wiki/pwr_dcjack_2.jpg)

# Enjoy! #