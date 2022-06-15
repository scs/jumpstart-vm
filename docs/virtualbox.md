Virtualbox
==========

Virtualbox funktioniert am besten, wenn kein Hyper-V oder Docker-Desktop installiert ist.
Sollte eines dieser Tools installiert sein, reduziert sich die Performance und Stabilität der VM,
da Virtualbox nicht direkt auf die Virtualisierungs-Hardware zugreifen kann.
In diesem Fall würde sich eine VM mit [Hyper-V](./hyperv.md) empfehlen.
Wenn man mit dessen Einschränkungen leben kann.


Installation
------------

Wir verwenden die neuste Version von Virtualbox as Hypervisor für die VM:
[VirtualBox Downloads](https://www.virtualbox.org/wiki/Downloads)

Nach der Installation von Virtualbox kann eine neue Linux-VM erstellt
und mit der Installation von Ubuntu begonnen werden.

* Genau dieses LTS Image von Ubuntu Desktop herunterladen: [ubuntu-desktop-22.04](https://ubuntu.com/download/desktop)
* In Virtualbox eine neue VM erstellen
  * Name: jumpstart-vm
  * Typ: Linux
  * Version: Ubuntu 64-bit
  * mind. 4GB RAM
  * mind. 64GB Harddisk
* VM konfigurieren: Ändern
  * Netzwerk -> Adapter 2 -> aktivieren -> Host-only Adapter
  * System -> Prozessor -> sinnvolle Anzahl wählen (maximal so viele wie Arbeitsspeicher in GB)
  * Anzeige -> Grafikspeicher: 128MB, 3D-Beschleunigung aktivieren
* VM starten
  * Ubuntu .iso als Boot-Medium anwählen
  * "Install Ubuntu"
  * "Minimal Installation"
  * "Erase Disk and install Ubuntu"
  * Computers name: jumpstart-vm
  * Username: vagrant
  * Password vagrant
* VM neu starten (Boot-Medium wird automatisch entfernt)
* Tools installieren
  * Konsole öffnen
  * `sudo apt update`
  * `sudo apt install -y git openssh-server`
  * Ab jetzt kann auch [headless](./headless.md) gearbeitet werden.
  * `git clone https://github.com/scs/jumpstart-vm.git`
  * `cd jumpstart-vm`
  * `./provision.sh`
  * Falls hier irgendwelche Fehler auftreten, einfach nochmals oder zuerst mit `sudo apt udpate && sudo apt upgrade -y` versuchen
* VM neu starten`


VM Tuning (optional)
---------

Bei Bedarf können Teilaspekte der VM auch nachträglich über das Virtualbox GUI angepasst werden.
Dazu muss die VM gestoppt sein.

* HDD Modus der virtuellen Disks auf SSD stellen.
* RAM/CPUs anpassen


Austauschen von Dateien zwischen VM und Host
--------------------------------------------


### Variante Copy-Paste

Die gewünschte Datei öffnen und den Inhalt mittels `Ctrl-A` und `Ctrl-C` kopieren.
Danach den Inhalt am Zielort in eine neue Datei einfügen.
Dazu muss im VM-Fenster unter: Geräte -> Gemeinsame Zwischenablage -> bidirektional gewählt sein

Achtung:
Unter Linux führt das Markieren von Text meist direkt zu einer Kopie dieses in der Zwischenablage.
Wenn also Inhalt vom Host in die VM kopiert werden soll,
sollte in der VM vorgängig kein Text markiert sein
und auch keiner vor dem Einfügen markiert werden.


### Variante SMB-Share

Die Desktop-VM hat bereits einen eingerichten Samba-Dienst,
welcher ein SMB-Share für das Home-Verzeichnis des `vagrant`-Users zur Verfügung stellt.
Dieses Share kann direkt in Windows verwendet werden.

* Öffnen des Dialogs für Netzwerk-Laufwerke mittels `Win` + `R` und:

  ~~~~~~
  RUNDLL32 SHELL32.DLL,SHHelpShortcuts_RunDLL Connect
  ~~~~~~

* Den zu verwendenden Laufwerkbuchstaben wählen
* Pfad für den Ordner: `\\jumpstart-vm\vagrant-home`
* `Verbindung bei Anmeldung wiederherstellen` und `Verbindung mit anderen Anmeldeinformationen herstellen` anwählen
* Im nächsten Dialog User: `\vagrant` (ohne Domäne) und Passwort: `vagrant` verwenden und `Anmeldedaten speichern` wählen


### Variante Gemeinsame Ordner

Unter "Gemeinsame Ordner" kann ein bind-Mount von einem Host-Pfad in die VM eingerichtet werden.
Dazu als Pfad den gewünschten Host-Ordner, einen beliebigen Namen und einen Einbindepunkt (z.B: /mnt/my_folder) angeben.
Die Option "Automatisch einbinden" sorgt dafür,
dass der gemeinsame Ordner bei jedem VM-Start erneut verbunden wird.
