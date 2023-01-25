## Raspberry PI 4 – Tuning & Overclocking & Tipps

+ Eine schnelle SDXD UHS-I Karte wie die San Disk Ulra 140MB/s

![](https://github.com/SteveMutter/autodarts-Community-Docs/blob/main/source/image6.png)

+ Aktive Kühlung

![](https://github.com/SteveMutter/autodarts-Community-Docs/blob/main/source/image7.png)

+ Unbedingt den Full Speed Kühlungsmodus benutzen

![](https://github.com/SteveMutter/autodarts-Community-Docs/blob/main/source/image8.png)

+ Installiere das ORIGINALE RASPBERRY OS (verwende den Imager, um di SDHC-Karte zu erstellen). Das ist sehr einfach. Du kannst die Desktop-Version laden (es spielt im Moment keine Rolle, ob du einen RPi4 mit 2GB oder 4GB RAM oder mehr hast). Du kannst auch die Konsolenversion laden (die Wahl liegt bei dir). Die Desktopversion über VNC ist später bequemer! Raspbian OS ist in Bezug auf GPU-Treiber viel besser optimiert als die Arch oder Ubuntu-Derivate.

```
https://www.raspberrypi.com/software/
```
![](https://github.com/SteveMutter/autodarts-Community-Docs/blob/main/source/image9.png)

###### Memory Split

Auf dem Raspberry Pi befindet sich ein SDRAM-Halbleiterspeicher, der als "shared memory" verwendet wird. Das bedeutet, dass der Hauptspeicher des CPU und der Grafikspeicher des GPU die Speicherkapazität des SDRAM-Chips teilen müssen. Auf der Softwareseite kann eingestellt werden, wie viel Speicher dem Grafikspeicher des GPU zugewiesen werden soll. Standardmäßig ist der GPU-Speicher auf 64 MB eingestellt. Der Rest von 256, 512 oder 1024 MB ist dann für den Hauptspeicher verfügbar. Es hängt davon ab, was mit dem Raspberry Pi gemacht wird. Im Grunde könnte man sagen, dass, wenn auf eine grafische Ausgabe verzichten werden kann, dann kann der Grafikspeicher auf 0 MByte eingestellt werden. Aber das ist nicht so einfach. Um den Raspberry Pi zu starten, benötigt die GPU mindestens 16 MB Grafikspeicher. Eine Einstellung von weniger als 16MB ist nicht zu empfehlen.

Wenn der Fenstermanager nicht benötigt wird, weil der Raspberry Pi als Server oder Gateway verwendet wird, kann man bedenkenlos den Speicher für die GPU reduzieren. Jedoch mindestens 16 MB.

Der Standardwert für Raspbian ist 64 MB. Wenn ein grafischer Fenstermanager, wie LXDE, auf dem Raspberry Pi verwendet wird, sollte daran nichts geändert werden. Das ist ein angemessener Wert, denn einige Funktionen können nicht verwendet werden, wenn nicht genug Grafikspeicher vorhanden ist.

Media Center wie Kodi funktionieren nur mit höheren Werten. Je nachdem, wie viel physischen Speicher mit 128 oder sogar 256 MB verfügbar ist. Sobald Video- oder grafikintensive Anwendungen zum einsatz kommen, wird dies empfohlen. Im Allgemeinen sollten die CPU-Kerne so viel RAM wie möglich haben. Wenn der GPU jedoch der Speicher ausgeht, werden Geschwindigkeitsprobleme auftreten. Zum Beispiel ruckelnde Videos.

> Memory Split mit dem einbebauten Konfigurations Tool einstellen:

```
sudo raspi-config
```

In “Advanced Options" unter dem Punkt "Memory Split" kann die Größe des Speichers eingestellt werden. Der kleinstmögliche Wert ist 16MB. Empfohlen werden 64MB (Die Destopversion läuft hier immernoch stabil).

> Memory Split via Konfig Datei:

```
sudo nano /boot/config.txt
```

Die Zeile mit "gpu_mem=" suchen und den Wert mit 64 austauschen
```
gpu_mem=64
```

Die Änderungen werden erst nach einem Neustart aktiv
```
sudo reboot now
```

###### OVERCLOCKING (Warnung! Overclocking kann die Garantie beeinflussen! Niemals ohne aktive Kühlung betreiben!)

Für Medium Einstellungen wird empfohlen folgendes in der Config zu ändern:
```
sudo nano /boot/config.txt
```

Die Zeilen "over_voltage=" and "arm_freq=" suchen und mit folgenden Werten ersetzen
```
over_voltage=6
arm_freq=2000
```

Für Maximale Performance folgende Werte anpassen:
```
force_turbo=1
over_voltage=8
arm_freq=2100
gpu_freq=750
```

Die Änderungen werden erst nach einem Neustart aktiv
```
sudo reboot now
```

Hinweis: Die Übertaktungseinstellungen sollten unter einer [all]-Anweisung oder - noch besser - unter dem entsprechenden Selektor für den Raspberry Pi stehen, z.B. [pi4]. Wird die SD Karte dann in einem anderen Pi verwendet so werden die Einstellungen nur bei kompatiblen Pi's ausgeführt.

![](https://github.com/SteveMutter/autodarts-Community-Docs/blob/main/source/image10.png)

Mehr Wissen zum Thema Overclocking:
```
https://buyzero.de/blogs/news/raspberry-pi-ubertakten-pi-4-pi-400-pi-3b
```

###### USB Spannungsbegrenzung

Wie bekannt ist, werden USB-Geräte fast ausnahmslos von dem USB-Anschluss mit Strom versorgt, an den sie angeschlossen sind. Egal ob USB-Festplatte, USB-Stick, WLAN-Adapter, Maus oder Tastatur. Was den Stromverbrauch betrifft, sind diese Geräte manchmal recht rücksichtslos, was bei den USB-Ports eines Standard-PCs kein Problem darstellt. Offiziell erlauben USB 1.1 nur 100 mA und USB 2.0 nur 500 mA. Trotz dieser Grenzen geben sich die Mainboard-Hersteller nicht die Mühe, schwache USB-Ports zu installieren. In der Praxis können USB-Geräte viel mehr als 100 oder 500 mA ziehen. Bei einem Raspberry Pi ist die Stromversorgung der USB-Ports eine knifflige Angelegenheit. Wenn Maus und Tastatur und vielleicht der eine oder andere USB-Stick angeschlossen sind, ist das kein Problem. Aber wenn Sie mit externen Festplatten oder stromhungrigen WLAN-Adaptern arbeiten, bricht die Stromversorgung des Raspberry Pi zusammen. Das hat nichts direkt mit dem Raspberry Pi zu tun, sondern mit seiner Stromversorgung über ein Steckernetzteil, das oft ein Ladegerät und keine echte Stromversorgung ist. Es ist schnell überfordert mit dem erheblichen Stromverbrauch mehrerer USB-Geräte. Aus diesem Grund wird generell empfohlen, dass USB-Geräte über einen aktiven USB-Hub mit dem Raspberry Pi verbunden werden. Ein aktiver USB-Hub hat eine eigene Stromversorgung, über die er die angeschlossenen Geräte mit Strom versorgen kann. Ab dem Raspberry Pi-Modell 
B+ ist die Stromversorgung deutlich stabiler, insbesondere über den USB für externe Geräte. Das B+-Modell hat auch einen Parameter eingeführt, der steuert, wie viel Strom USB-Geräte aus dem USB-Port insgesamt ziehen dürfen. Standardmäßig ist der Gesamtstrom von den USB-Ports auf 600 mA begrenzt. Diese Grenze besteht dazu, um zu verhindern, dass der Raspberry Pi instabil wird und bei einem stromhungrigen USB-Gerät abschaltet. In diesem Fall wird nur der USB ausgeschaltet. Insgesamt darf der Raspberry Pi den USB-Geräten 1,2 A (1.200 mA) zur Verfügung stellen, wenn die Grenze entfernt wird.

Konfigfile öffnen:
```
sudo nano /boot/config.txt
```

Nach der Zeile "max_usb_current=" suchen und den Wert anpassen
```
sudo nano max_usb_current=1
```

Die Änderungen werden erst nach einem Neustart aktiv
```
sudo reboot now
```

###### Von einer SSD Booten

Um den Raspberry Pi so zu konfigurieren, dass er von einem USB-Laufwerk startet, muss er einmal mit Raspbian von einer SD-Karte gestartet werden.

Konfigfile öffnen:
```
sudo nano /boot/config.txt
```

Nach der Zeile "program_usb_boot_mode=" suchen und den Wert anpassen
```
program_usb_boot_mode=1
```
> Nun ist es wichtig den Raspi einmal neu zu starten damit die Änderungen aktiv werden.

```
sudo reboot now
```

Wenn der Raspi wieder hochgefahren ist mit folgendem Befehl überprüfen ob die Einstellung angewendet wurde:
```
vcgencmd otp_dump | grep 17:
```

Die Ausgabe sollte sein: 
```
17:3020000a
```
Dann kann der Raspi ausgeschaltet werden und die SD Karte herausgenommen werden.

> Vorbereiten des USB Speichers

Normalerweise bootet der Pi von einer SD Karte. Um von einem USB Speicher booten zu können muss das entsprechene OS auf den USB Speicher geschrieben werden. z.B. mit dem Raspberry Pi Imager. Der USB Speicher wird dann an einem freien USB Port angeschlossen. Es ist wichtig das beim starten von einem USB Speicher keine Speicherkarte im Raspi steckt. Zur Fehleranalyse sollte ein Monitor angeschlossen sein. Bei externen Fesplatten kann ein fehler sein wenn diese zu viel Strom über den USB Port zieht oder der Laufwerkscontoller zu lange braucht, um zu starten. Für beide Probleme gibt es bereits Lösungen.
