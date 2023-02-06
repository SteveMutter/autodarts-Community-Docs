## Hier findest du vorkonfigurierte Images für den Raspberry Pi

[x] autodarts vorinstalliert

[x] guvcview vorinstalliert

[x] VNC aktiviert

[x] Startup script um guvcview automatisch zu starten und Profile zu laden

[x] Offnet den Chromium Browser im Vollbildmodus / htts://autodarts.io

[x] Benutzername/Passwort für alle Images autodarts/autodarts

[X] Bluetooth ist deaktiviert


## Download

Raspberry Pi OS Desktop (Legacy) / Debian Buster / 32Bit
```
https://tinyurl.com/2zrtauf9
```

Raspberry Pi OS Desktop / Debian Bullseye / 64Bit
```
https://tinyurl.com/nhh3ebd4
```



## Benutzung des Images

Das Image sollte mit dem Raspberry Pi Imager auf eine SD Karte geschrieben werden.

## Erste Schritte

Nachdem der Raspberry Pi gebootet ist öffnet sich guvcview und Chromium im Vollbildmodus (Das kann etwas dauern, da vor dem Öffnen auf eine aktive Internetverbindung gewartet wird)

Im ersten Schritt richtet bitte eine Internetverbindung ein (LAN oder WLAN) und startet den Pi neu


Wir navigieren zum Board Manager
```
localhost:3180
```

Jetzt autodarts eingerichtet werden

> Hierzu gibt es ein Video
```
https://invidious.drivet.xyz/wQxRMKd2K6o?t=223
```

> Bluetooth wieder aktivieren

```
sudo nano /boot/config.txt
```

> Die letzte Zeile löschen
```
dtoverlay=disable-bt
```

> Den Service wieder aktivieren
```
sudo systemctl enable hciuart
```

> und
```
sudo systemctl enable bluetooth.service
```

> Überprüfen ob der Service fehlerfrei läuft
```
hciconfig
```

> Ausgabe:
```
hci0:	Type: Primary  Bus: UART
	BD Address: DC:A6:32:D8:88:59  ACL MTU: 1021:8  SCO MTU: 64:1
	UP RUNNING 
	RX bytes:2772 acl:0 sco:0 events:172 errors:0
	TX bytes:5826 acl:0 sco:0 commands:150 errors:0
```
