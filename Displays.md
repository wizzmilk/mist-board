# List of displays tested with the MiST board #

Since the MiST FPGA cores have a video timing thats closely derived from the
video timing of the original machines the resulting modes are not 100% VGA
compatible. Different screens cope differently with this. The results range
from completely unusable screens to screens that display nearly everything perfectly.

Especially all PAL modes with a vertical scan rate of 50Hz are exceeding the VGA specification which requires at least 56Hz. Some screens even support a horizontal scan rate of 15khz (TV) modes although this is way below the VGA requirement of 31.5kHz.

Please note in the comment column of single cores/video modes are not supported.

| **Display** | **size** | **Rating** | **50Hz** | **15kHz** | **Comments** |
|:------------|:---------|:-----------|:---------|:----------|:-------------|
| Eizo Flexscan L568 | 17" 1280x1024 | 95%        | Yes      | Yes       | permanent "signal out of range" message on 15kHz modes, works anyway |
| Dell 1908FP | 19" 1280x1024 | 90%        | Yes      | No        |              |
| NEC LCD 1525V | 15" 1024x768 | 40%        | No       | No        |              |


# HDMI / upscaling options #

Using a video upscaler increases compatibility significantly, as HDMI input is generally good on most displays / TVs.

| **Device** | **Rating** | **Output** | **50Hz** | **15kHz RGBHV** | **Comments** | **Example** |
|:-----------|:-----------|:-----------|:---------|:----------------|:-------------|:------------|
| Micomsoft XRGB-Mini + Arcadeforge UMSA | 99%        | HDMI       | Yes      | Yes             | 15khz mode works great. 50hz mode has pink tint on C64, MSX, PCE cores. | [video](https://www.youtube.com/watch?v=1uN4RDPfnKc) |
| Gonbes 8220 RGB to VGA PCB  | 80%        | VGA        | Yes      | No              | Most cores work, see [Core status sheet](https://docs.google.com/spreadsheets/d/1P1LPndBDkcSSG9Cxco8NGMnihYyO-4FIvz1kcyZ7vi4/). | -           |
| VGA+R/L TO HDMI adapter - HD2022 | 70%        | HDMI       | Yes      | No              | Looks same as VGA output but incompatible with upscalers |             |
| MINI VGA2HDMI | 70%        | HDMI       | Yes      | No              | uses same converter chip as the above adapter |             |


# Gonbes Notes #
This board can upscale the image to 800x600 or 1024x768. At 640x480, it solves most scaling problems seen on several displays, so this card is a good way to make the MiST more compatible with many TVs. Caveat of this board is that many cores show an image off-centered, hiding some of the gameplay areas.


# XRGB Notes #
You will need a VGA (RGBHV) to SCART adapter to connect to this upscaler (making sure your connection to the scaler uses the right type of SCART, e.g. usually Euro and not JP21). Can upscale signal to 720p or 1080p and convert 50hz to 60hz smoothly.
Sometimes the unit has trouble switching from 50 to 60hz or vice-versa, and will show a blank screen (or blue depending on settings). Restarting the XRGB will fix it.