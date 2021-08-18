jumpstart-vm
============

Für den [Jumpstart-Kurs](https://github.com/scs/jumpstart-docs) wird eine Entwicklung-Umgebung benötigt,
in welcher alle Tools für die entsprechenden Übungs-Aufgaben installiert sind.
Damit diese Umgebung nicht mit anderen bereits installierten Tools kollidiert
und für alle Teilnehmen konsistent ist,
wird dafür eine virtuelle Machine eingerichtet.

Als Basis wird Ubuntu-Desktop in einer `virtualbox` VM verwendet,
auf welcher mittels `ansible` im Folgenden alle benötigten Tools automatisiert installiert werden.


Installation
------------

Wir verwenden die neuste Version von Virtualbox as Hypervisor für die VM:
[VirtualBox Downloads](https://www.virtualbox.org/wiki/Downloads)

Nach der Installation von Virtualbox kann eine neue Linux-VM erstellt
und mit der Installation von Ubuntu begonnen werden.

* Neustes LTS Image von Ubuntu Desktop herunterladen: [ubuntu-desktop-20.04](https://ubuntu.com/download/desktop)
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
  * `sudo apt install -y git openssh-server`
  * `git clone https://github.com/scs/jumpstart-vm.git`
  * `cd jumpstart-vm`
  * `./provision.sh`
  * Falls hier irgendwelche Fehler auftreten, einfach nochmals oder zuerst mit `apt udpate && apt upgrade -y` versuchen
* VM neu starten


Rechen-Performance der VM
-------------------------

Damit die VM beinahe native Rechen-Leistung erreicht,
sollte sicherstellt werden,
dass der Rechner alle benötigten Virtualisierung-Technologien aktiviert hat.
Diese finden sich in den BIOS/UEFI-Einstellungen.

Unter Intel-CPUs: `VT-x` und `VT-d`

Unter AMD-CPUs: `AMD-V` und `AMD-Vi` oder `SVM`


VM Tuning (optional)
---------

Bei Bedarf können Teilaspekte der VM auch nachträglich über das Virtualbox GUI angepasst werden.
Dazu muss die VM gestoppt sein.

* HDD Modus der virtuellen Disks auf SSD stellen.
* RAM/CPUs anpassen


Verwendung der Desktop-VM
=========================

Die VM kann direkt im Virtualbox GUI gestartet werden
und die gewohnte simulierte Desktop-Oberfläche verwendet werden.
Nutzername und Passwort sind: `vagrant:vagrant`.


Arbeiten mit JetBrains IDEs
---------------------------

Alle JetBrains-Produkte können entweder gratis (Community-Edition) oder als Test-Version (30 Tage) verwendet werden.

Die meisten IDEs verfügen über ausführliche Hotkey-Systeme,
die das Arbeiten erheblich erleichtern.
Folgend ein kleiner Auszug aus CLion:

| Shortcut               | Explanation                      |
|---                     |---                               |
| `Ctrl` + `Alt` + `l`   | Automatische Formatierung        |
| `Ctrl` + `F9`          | Build                            |
| `Shift` + `F9`         | Run                              |
| `Alt` + `F7`           | Find usage                       |
| `Shift`, `Shift`       | Intelligente Suche               |
| `Ctrl` + `Alt` + `s`   | Settings                         |
| `Ctrl` + `Shift` + `f` | Search all                       |
| `Ctrl` + `Shift` + `r` | Search & Replace all             |

* [CLion Reference Card](https://resources.jetbrains.com/storage/products/clion/docs/CLion_ReferenceCard.pdf)
* [IntelliJ Reference Card](https://resources.jetbrains.com/storage/products/intellij-idea/docs/IntelliJIDEA_ReferenceCard.pdf)


Weitere verfügbare Tools
------------------------

* `git gui` (git Commit Helper)
* `gitk` (git Tree Viewer)
* `valgrind` (Memory Analyzer)
* `nano` (Console Editor)
* `tmux` (Shell Multiplexer)
* `okular` (PDF Viewer)
* `fdfind` (fast search tool)


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


Headless-Verwendung
-------------------

Die VM ist auch für die Benutzung ohne Desktop-Oberflache und Virtualbox-GUI ausgelegt.
Als Alternative kann mittels SSH und X11-Forwarding gearbeitet werden.
Weitere Infos dazu unter: [docs/headless_and_vagrant.md](docs/headless_and_vagrant.md)
