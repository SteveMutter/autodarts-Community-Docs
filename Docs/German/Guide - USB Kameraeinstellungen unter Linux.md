## Manuelle USB-Kameraeinstellungen in Linux

Es gibt viele UVC-kompatible Webcams, die meisten unterstützen den vollautomatischen Modus, aber nur wenige dieser Kameras bieten erweiterte manuelle Weißabgleich-, Gain- und Belichtungssteuerung. Beispiel-CAMS: Kurokesu C1-Familie (C1, C1 PRO, C1 MICRO), Logitech C920 und Brio sind diejenigen, auf die man sich verlassen kann.

Es gibt nur wenige Tools, um mit USB-Kameras in Linux zu arbeiten. V4L2-CTL ist wahrscheinlich das fortschrittlichste und empfohlene Befehlszeilen-Tool für fortgeschrittene Benutzer.

Ein frisches Linux kann die Utility fehlen, installieren Sie es mit einem einfachen Befehl:

```
sudo apt update
sudo apt-get install v4l-utils
```

Lassen Sie uns jetzt sehen, was wir an einem USB-Port haben

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

Beachten Sie zwei Schnittstellen für die Kurokesu C1-Kamera - dies ist eine Dual-Stream-Ausgabe. Ein Video-Gerät ist für die reguläre YUYV / MJPEG komprimierte Ausgabe, ein anderes für h.264. Sie können beide gleichzeitig mit verschiedenen Programmen öffnen (z.B. h.264 für Live-Streaming, MJPG für Onboard-Aufzeichnung oder Computer-Vision-Verarbeitung)

++ Liste verfügbarer Steuerelemente

Video for Linux V4L2 kann alle verfügbaren Steuerelemente in eine Liste melden. Die Liste ist selbsterklärend mit möglichen Wertbereichen.

```
v4l2-ctl -d /dev/video0 --list-ctrls
```

## Kurokesu C1-Familie

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

##Aktuelle Werte auslesen

Lesen Sie den aktuellen UVC-Wert mit dem untenstehenden Befehl. Der tatsächliche Wert wird zurückgegeben
```
v4l2-ctl --get-ctrl=white_balance_temperature
```
```
white_balance_temperature: 4600
```

## Neuen Wert festlegen

Um einen neuen Kameraparameterwert festzulegen, verwenden Sie eine Befehlssyntax wie diese:

```
v4l2-ctl --set-ctrl=gain=00
```
```
v4l2-ctl --set-ctrl=exposure_auto=1
```
```
v4l2-ctl --set-ctrl=exposure_absolute=10
```

In manchen Fällen ist es notwendig, Parameter in bestimmter Reihenfolge umzuschalten, damit der Parameter festgelegt werden kann. Zum Beispiel ändern Sie die manuelle Weißabgleichsteuerung, bevor Sie einen festen Wert festlegen:

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

## Kurokesu C1 Erweiterungseinheit und Firmwareanpassung

Neben den standardmäßigen UVC-Steuerelementen ist es möglich, die Firmware der Kurokesu C1-Familie der Kamera an die spezifischen Kundenbedürfnisse anzupassen, zum Beispiel:

+ Standardwerte für UVC-Steuerelemente festlegen
+ De-Noise-Parameter bearbeiten
+ USB-Framesize bearbeiten
+ MJPEG-Komprimierungsrate bearbeiten
+ H.264-Bitrate bearbeiten
+ Verhalten bei schlechten Lichtverhältnissen festlegen (automatisch auf 2 Bilder pro Sekunde reduzieren oder stabile Bildrate aufrechterhalten)
+ und vieles mehr

Die Kamera-Erweiterungseinheit ist für Linux konzipiert und ermöglicht es, noch mehr Steuerelemente zu ändern, die nicht von dem UVC-Protokoll abgedeckt werden.