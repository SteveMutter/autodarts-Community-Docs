## Configure and autostart guvcview
## Only works on x32 System

###### Create Profiles and save them

> Requirements:
> Running guvcview without any Issues

First of all we configure guvcview

+ Start guvcview in Guimode via Terminal:

```
guvcview
```

guvcview should open in GUI mode

![](https://github.com/SteveMutter/autodarts-Community-Docs/blob/main/source/image11.png)

+ Select your Firts Camera and do your Settings

![](https://github.com/SteveMutter/autodarts-Community-Docs/blob/main/source/image12.png)

+ Save this Profile with an unique name e.g. video0

+ Do the same for Camera 2 and 3

+ Now we got 3 Profiles one for each camera

###### Building script

Now we build a script that launches guvcview for each camera with it's profile

We build the script via Terminal so open up a Terminal window


Now we create the script via nano editor and store it in Documents folder

```
sudo nano /home/autodarts/Documents/guvcviewstart.sh
```
> note: in this case "autodarts" is the user folder / change this with your local user

 within the script we type following
```
#!/bin/sh
guvcview -z -d /dev/video0 -p /home/Documents/video0 &
guvcview -z -d /dev/video2 -p /home/Documents/video2 &
guvcview -z -d /dev/video4 -p /home/Documents/video4
exit0
```

> parameter "-z" opens guvcview in GUI Mode without webcam view

> parameter "-d" calls the unique device

> parameter "-p" calls the profile for the device

+ We store the file by hitting "Ctrl + X" / "Y" / "Return"

+ Now we make the file executable
```
sudo chmod -x /home/autodarts/Documents/guvcviewstart.sh
```

+ The script can be tested with
```
sh /home/autodarts/Documents/guvcviewstart.sh
```

guvcview should open 3 windows one for each cam / without view of webcam

Now you can configure your cameras via this 3 windows, you should see the changes directly in Board Manager Live View


###### start the script on boot ( Raspberry Pi 32bit Legacy )

There are planty of ways to autstart sh scripts. We use the LXDE way

 Open up a terminal window and open the autostart file

```
sudo nano /etc/xdg/lxsession/LXDE-pi/autostart
```

 Add the following to the end of the file
```
@/home/autodarts/Documents/guvcviewstart.sh
```

![](https://github.com/SteveMutter/autodarts-Community-Docs/blob/main/source/image13.png)

Now you can test it by rebooting your Pi

```
sudo reboot now
```


###### start the script on boot (Raspberry Pi 64bit)

Make new directory
```
sudo mkdir /home/autodarts/.config/autostart
```

Make a new file in this directory
```
sudo nano /home/autodarts/.config/autostart/autostart.desktop
```

Write following in this file:
```
#!usr/bin/env xdg-open
[Desktop Entry]
Version=1.0
Name=Autorun
Type=Application
Exec=/bin/sh /home/autodarts/Documents/scripts/autostart.sh &
Terminal=false
```

Change Permission 
```
sudo chown autodarts /home/autodarts/.config/autostart/autostart.desktop
sudo chgrp autodarts /home/autodarts/.config/autostart/autostart.desktop
sudo chmod 744 /home/autodarts/.config/autostart/autostart.desktop
```

Now you can test it by rebooting your Pi

```
sudo reboot now
```
