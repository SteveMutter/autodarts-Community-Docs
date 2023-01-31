## Here you can find some Prebuild Images

[x] autodarts installed

[x] guvcview installed

[x] VNC active

[x] Startup script to start guvcview on boot and load settings from Profiles

[x] Open Chromium Browser in Fullscreen mode at htts://autodarts.io

[x] username/password for all Images are autodarts/autodarts

[X] Bluetooth is disabled


## Download

Raspberry Pi OS Desktop (Legacy) / Debian Buster / 32Bit
```
https://tinyurl.com/amnyunxr
```

Raspberry Pi OS Desktop / Debian Bullseye / 64Bit
```
https://tinyurl.com/6cyr2xpu
```


## Usage

The Image should be written to an SD card with Raspberry Pi Imager

## First Steps

After the Pi is started guvcview and Chromium will open (This could take some time cause the script waits for an active Internetconnection)

For the first time you can close all open Applications and set up an Connection to your network (Cabled or Wifi doesn't matter) then restart the Pi


Open Chromium and navigate to
```
localhost:3180
```

Now autodarts can be configured

> There is an video on how to do this
```
https://invidious.drivet.xyz/wQxRMKd2K6o?t=223
```
> activate Bluetooth again

```
sudo nano /boot/config.txt
```

> delete last column
```
dtoverlay=disable-bt
```

> activate the services
```
sudo systemctl enable hciuart
```

> and
```
sudo systemctl enable bluetooth.service
```

> check if service are running
```
hciconfig
```

> Output:
```
hci0:	Type: Primary  Bus: UART
	BD Address: DC:A6:32:D8:88:59  ACL MTU: 1021:8  SCO MTU: 64:1
	UP RUNNING 
	RX bytes:2772 acl:0 sco:0 events:172 errors:0
	TX bytes:5826 acl:0 sco:0 commands:150 errors:0
```