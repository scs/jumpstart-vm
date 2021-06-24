jumpstart-vm
============

Für die Teilnehmenden existiert eine vorbereitete Desktop-VM,
welche direkt verwendet werden kann.
Die VM selber ist bereits generiert
und muss nur noch in Virtualbox importiert werden.

Das Archiv-File der VM befindet sich auf dem AD oder dem verteilten USB-Stick.


Virtualbox
----------

Wir verwenden Virtualbox as Hypervisor für die VM.
[VirtualBox Downloads](https://www.virtualbox.org/wiki/Downloads)

Nach der Installation kann die heruntergeladene VM als Appliance importieren.


Rechen-Performance der VM
-------------------------

Damit die VM beinahe native Rechen-Leistung erreicht,
sollte sicherstellt werden,
dass der Rechner alle benötigten Virtualisierung-Technologien aktiviert hat.
Diese finden sich in den BIOS/UEFI-Einstellungen.

Unter Intel-CPUs: `VT-x` und `VT-d`

Unter AMD-CPUs: `AMD-V` und `AMD-Vi` oder `SVM`

Sollten trotzdem Probleme mit der VM auftreten,
sollte sichergestellt werden,
dass KVM als Paravirtualisierung in Virtualbox eingestellt ist.


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
* Im nächsten Dialog User/Passwort: `vagrant:vagrant` verwenden und `Anmeldedaten speichern` wählen
