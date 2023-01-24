## guvcview Konfigurieren und beim Boot automatisch starten

###### Profile generieren und speichern

> Vorraussetzungen:
> guvcview ist installiert und läuft reibungslos

Als erstes konfigurieren wir guvcview

+ guvcview via Terminal starten:

```
guvcview
```

guvcview sollte sich im GUI Modus öffnen

![](https://github.com/SteveMutter/autodarts-Community-Docs/blob/main/source/image11.png)

+ Erste Kamera auswählen und einstellen

![](https://github.com/SteveMutter/autodarts-Community-Docs/blob/main/source/image12.png)

+ Das Profil unter einem eigenen Namen speichern / z.B. video0

+ Das gleiche für Kamera 2 und 3 machen

+ Nun haben wir insgesamt 3 Profile für jede Kamera eines

###### Startscript erstellen

Jetzt erstellen wir ein Startscript das guvcview im GUI Modus öffnet und die Profile lädt

Öffne ein Terminal Fenster


Wir benutzen den nano Editor und speichern das Script im Documents Ordner ab

```
sudo nano /home/autodarts/Documents/guvcviewstart.sh
```
> Achtung: In diesem Fall steht autodarts für das Benutzer Verzeichnis. Ersetze es mit deinem Benutzernamen z.B. /home/peter/Documents

 In die neue Datei schreiben wir folgende Befehle
```
#!/bin/sh
guvcview -z -d /dev/video0 -p /home/Documents/video0 &
guvcview -z -d /dev/video2 -p /home/Documents/video2 &
guvcview -z -d /dev/video4 -p /home/Documents/video4
exit0
```

> parameter "-z" öffnet guvcview im GUI Modus ohne Webcamfenster

> parameter "-d" definiert das Gerät

> parameter "-p" definiert das Profil

+ Wir speichern unser Script mit "Strg + X" / "Y" / "Enter"

+ Nun müssen wir die Datei ausführbar machen
```
sudo chmod -x /home/autodarts/Documents/guvcviewstart.sh
```

+ Jetzt können wir unser Script testen
```
sh /home/autodarts/Documents/guvcviewstart.sh
```

guvcview sollte sich 3x im GUI Modus öffnen / es wird kein Livebild der Kameras angezeigt

Nun kannst du deine Webcams einstellen. Die Ansicht sollte nun über den Live View im Board Manager möglich sein / Speichern der Profile nicht vergessen


###### Das Script beim Start ausführen

Es gibt viele Wege um das Script beim Start auszuführen. Wir nehmen den Weg über LXDE

Öffne die Autostart Datei

```
sudo nano /etc/xdg/lxsession/LXDE-pi/autostart
```

Füge folgendes am Ende der Datei hinzu
```
@/home/autodarts/Documents/guvcviewstart.sh
```

![](https://github.com/SteveMutter/autodarts-Community-Docs/blob/main/source/image13.png)

Nun können wir alles testen indem wir den Raspberry neustarten

```
sudo reboot now
```

