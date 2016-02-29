A few users have asked for a real time clock to be used with the Atari
ST core. While the board itself does not provide something like that
it's luckily pretty exandable with USB peripherals.

Unfortunately something like a USB RTC doesn't seem to exist, so i had
to build my own. This is a rather easy task given the fact that many
years ago i built a usb to i2c converter named  [I2C-Tiny-USB](http://www.harbaum.org/till/i2c_tiny_usb/). This device can easily be combined with one of those cheap DS1307 based i2c RTCs available from EBay. The resulting device looks like this:

![http://mist-board.googlecode.com/svn/wiki/usbrtc_foto.jpg](http://mist-board.googlecode.com/svn/wiki/usbrtc_foto.jpg)

### Using the device on the MiST ###

The latest firmware supports this device out of the box. Just plug it in and it will be used. No configuration required.

### Using the device under Linux ###

The device runs out of the box under Linux. The i2c-tiny-usb is detected by nearly all Linux distros out of the box. Also the ds1307 is supported. The only link missing is to tell Linux to probe for the RTC as the i2c doesn't have a auto detection mechanism.

Once the i2c-tiny-usb has been plugged in enter "dmesg" to check for the proper detection of the i2c bus. Dmesg should show something like this:

```
[10011.794901] usb 2-1.4: USB disconnect, device number 9
[10241.628102] usb 2-1.4: new low-speed USB device number 10 using ehci-pci
[10241.746228] usb 2-1.4: New USB device found, idVendor=0403, idProduct=c631
[10241.746233] usb 2-1.4: New USB device strings: Mfr=1, Product=2, SerialNumber=0
[10241.746237] usb 2-1.4: Product: i2c-tiny-usb
[10241.746240] usb 2-1.4: Manufacturer: Till Harbaum
[10241.746770] i2c-tiny-usb 2-1.4:1.0: version 1.05 found at bus 002 address 010
[10241.747272] i2c i2c-5: connected i2c-tiny-usb device
```

Now that we know the i2c bus has the id i2c-5 we can request the driver for the ds1307 to be loaded using the following command (replace the bus id with the one you just found out) as root:

```
echo ds1307 0x68 > /sys/bus/i2c/devices/i2c-5/new_device 
```

Once more check "dmesg" for additional information and you should find something like this:

```
[10241.813427] rtc-ds1307 5-0068: rtc core: registered ds1307 as rtc1
[10241.813434] rtc-ds1307 5-0068: 56 bytes nvram
[10241.813492] i2c i2c-5: new_device: Instantiated device ds1307 at 0x68
```

Your RTC is detected. In this case as rtc1 since the system already had a built-in rtc used under rtc0.

You can now read the time stored in the rtc using:

```
$ sudo hwclock -f /dev/rtc1 -r
Tue 20 May 2014 12:19:17 PM CEST  
```

Or you can set the current system time to the RTC using the following command:

```
hwclock -f /dev/rtc1 -w
```