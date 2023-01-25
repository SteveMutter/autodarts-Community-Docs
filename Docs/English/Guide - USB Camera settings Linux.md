## Manual USB camera settings in Linux

There are many UVC compatible webcams most of them support full auto mode but only a
few of these cameras provide extended manual white balance, gain and exposure control.
EXAMPLE CAMS: Kurokesu C1 family (C1, C1 PRO, C1 MICRO), Logitech C920 and Brio are the ones that can be trusted.

There are few tools to work with USB cameras in Linux. Probably V4L2-CTL is the most
advanced and recommended command line tool for advanced users.

Fresh Linux might be missing utility, install it with simple command:

```
sudo apt update
sudo apt-get install v4l-utils
```

Now let’s see what we have connected on USB port

```
v4l2-ctl --list-devices
```
```
KurokesuC1_SN000803 (usb-0000:00:14.0-1):
/dev/video0
/dev/video1
```
```
HD Pro Webcam C920 (usb-3f980000.usb-1.2):
/dev/video2
```

Note two interfaces for Kurokesu C1 camera – this is dual stream output. One video device is
for regular YUYV/MJPEG compressed output another is for h.264. You can open both of
them at the same time with different programs (for example h.264 for live streaming, MJPG
for onboard recording or computer vision processing)

## List available controls

Video for Linux V4L2 can report all available controls to single list. List is self explanatory
with possible value ranges.

```
v4l2-ctl -d /dev/video0 --list-ctrls
```

## Kurokesu C1 family

```
brightness 0x00980900 (int): min=-64 max=64 step=1 default=0 value=0
contrast 0x00980901 (int): min=0 max=64 step=1 default=32 value=32
saturation 0x00980902 (int): min=0 max=128 step=1 default=64 value=64
hue 0x00980903 (int): min=-40 max=40 step=1 default=0 value=0
white_balance_temperature_auto 0x0098090c (bool): default=1 value=1
gamma 0x00980910 (int): min=72 max=500 step=1 default=100 value=100
gain 0x00980913 (int): min=0 max=100 step=1 default=0 value=0
power_line_frequency 0x00980918 (menu): min=0 max=2 default=1 value=1
white_balance_temperature 0x0098091a (int): min=2800 max=9300 step=1 default=4600 value=4600 flags=inactive
sharpness 0x0098091b (int): min=0 max=6 step=1 default=3 value=3
backlight_compensation 0x0098091c (int): min=0 max=2 step=1 default=1 value=1
exposure_auto 0x009a0901 (menu): min=0 max=3 default=3 value=3
exposure_absolute 0x009a0902 (int): min=1 max=10000 step=1 default=156 value=156 flags=inactive
exposure_auto_priority 0x009a0903 (bool): default=0 value=0
```

## Logitech C920

```
brightness (int): min=0 max=255 step=1 default=-8193 value=128
contrast (int): min=0 max=255 step=1 default=57343 value=128
saturation (int): min=0 max=255 step=1 default=57343 value=128
white_balance_temperature_auto (bool): default=1 value=1
gain (int): min=0 max=255 step=1 default=57343 value=0
power_line_frequency (menu): min=0 max=2 default=2 value=2
white_balance_temperature (int): min=2000 max=6500 step=1 default=57343 value=4000 flags=inactive
sharpness (int): min=0 max=255 step=1 default=57343 value=128
backlight_compensation (int): min=0 max=1 step=1 default=57343 value=0
exposure_auto (menu): min=0 max=3 default=0 value=3
exposure_absolute (int): min=3 max=2047 step=1 default=250 value=250 flags=inactive
exposure_auto_priority (bool): default=0 value=1 
pan_absolute (int): min=-36000 max=36000 step=3600 default=0 value=0
tilt_absolute (int): min=-36000 max=36000 step=3600 default=0 value=0
focus_absolute (int): min=0 max=250 step=5 default=8189 value=0 flags=inactive
focus_auto (bool): default=1 value=1
zoom_absolute (int): min=100 max=500 step=1 default=57343 value=100
```

## Logitech Brio 4K

```
brightness (int): min=0 max=255 step=1 default=128
contrast (int): min=0 max=255 step=1 default=128
saturation (int): min=0 max=255 step=1 default=128 value=128
white_balance_temperature_auto (bool): default=1 value=1
gain (int): min=0 max=255 step=1 default=0 value=0
power_line_frequency (menu): min=0 max=2 default=2 value=2
white_balance_temperature (int): min=2000 max=7500 step=10
default=4000 value=2770 flags=inactive
sharpness (int): min=0 max=255 step=1 default=128 value=128
backlight_compensation (int): min=0 max=1 step=1 default=1 value=1
exposure_auto (menu): min=0 max=3 default=3 value=3
exposure_absolute (int): min=3 max=2047 step=1 default=250 value=625 flags=inactive
exposure_auto_priority (bool): default=0 value=1
pan_absolute (int): min=-36000 max=36000 step=3600 default=0 value=0
tilt_absolute (int): min=-36000 max=36000 step=3600 default=0 value=0
focus_absolute (int): min=0 max=255 step=5 default=0 value=20 flags=inactive
focus_auto (bool): default=1 value=1
zoom_absolute (int): min=100 max=500 step=1 default=100 value=100
led1_mode (menu): min=0 max=3 default=0 value=3
led1_frequency (int): min=0 max=255 step=1 default=0 value=0
```

## Reading current value

Read current UVC value with command below. Actual value is reported back below.
```
v4l2-ctl --get-ctrl=white_balance_temperature
```
```
white_balance_temperature: 4600
```

## Setting new value

In order to set new camera parameter value use command syntax like this:

```
v4l2-ctl --set-ctrl=gain=00
```
```
v4l2-ctl --set-ctrl=exposure_auto=1
```
```
v4l2-ctl --set-ctrl=exposure_absolute=10
```


In some cases it is necessary to switch parameters in certain order for parameter to be set. For
example change manual white balance control before setting fixed value:

```
v4l2-ctl --set-ctrl=white_balance_temperature_auto=1
```
```
v4l2-ctl --set-ctrl=white_balance_temperature=6000
```
```
VIDIOC_S_CTRL: failed: Input/output error
```
```
white_balance_temperature: Input/output error
```
```
v4l2-ctl --set-ctrl=white_balance_temperature_auto=0
```
```
v4l2-ctl --set-ctrl=white_balance_temperature=6000
```

## Kurokesu C1 extension unit and firmware customization

Besides standard UVC control it is possible to customize Kurokesu C1 family camera
firmware to meet specific customer needs, for example:

+ Set default UVC control values
+ Edit de-noise parameters
+ Edit USB frame size
+ Edit MJPEG compression rate
+ Edit h.264 bitrate
+ Set low light behavior (lower down to 2 frames per second automatically or maintain stable frame rate)
+ and much more

Camera Extension Unit is designed for Linux and allows to alter even more controls not
covered by UVC protocol (for example: spot/center/frame measuring mode).