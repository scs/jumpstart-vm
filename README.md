jumpstart-vm
============

Für die Teilnehmer existiert eine vorgenerierte Desktop-VM,
welche direkt verwendet werden kann.
Die VM selber ist bereits generiert
und muss nur noch in Virtualbox importiert werden.

Das Archiv-File der VM finden Sie auf dem AD oder auf dem verteilten USB-Stick.


Virtualbox
----------

Wir verwenden Virtualbox as Hypervisor für die VM.
[VirtualBox Downloads](https://www.virtualbox.org/wiki/Downloads)

Nach der Installation können Sie die heruntergeladene VM als Appliance importieren.


Rechen-Performance der VM
-------------------------

Damit die VM beinahe native Rechen-Leistung erreicht,
sollten Sie sicherstellen, dass Ihr Rechner alle benötigten Virtualisierungs-Technologien
aktiviert hat. Diese finden Sie in den BIOS/UEFI-Einstellungen ihres PCs.

Unter Intel-CPUs aktivieren Sie: `VT-x` und `VT-d`

Unter AMD-CPUs aktivieren Sie: `AMD-V` und `AMD-Vi`

Sollten trotzdem Probleme mit der VM auftretten,
stellen Sie sicher, dass KVM als Paravirtualisierung in Virtualbox eingestellt ist.


VM Tuning (optional)
---------

Bei Bedarf können Teilaspekte der VM auch nachträglich über das Virtualbox GUI
angepasst werden. Dazu muss die VM gestoppt sein.

* HDD Modus der virtuellen Disks auf SSD stellen.
* RAM/CPUs anpassen


Verwendung der Desktop-VM
=========================

Starten Sie die VM direkt in Virtualbox und verwenden Sie die Desktop-Oberfläche.
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

Öffnen Sie die entsprechende Datei und kopieren Sie den Inhalt mittels
`Ctrl-A` und `Ctrl-C`. Fügen Sie den Inhalt nun am Zielort in eine
neue Datei ein.

Achtung: Unter Linux führt das Markieren von Text meist direkt zu einer Kopie dieses
in der Zwischenablage. Wenn Sie also vom Host in die VM kopieren möchten,
sollten Sie in der VM keinen Text bereits markiert haben und auch keinen vor dem
Einfügen markieren.


### Variante SMB-Share

Die Desktop-VM hat bereits einen eingerichten Samba-Dienst,
welcher ein SMB-Share für das Home-Verzeichnis des `vagrant`-Users zur Verfügung stellt.
Dieses Share können Sie in Windows direkt verwenden.

* Öffnen Sie den Dialog für Netzwerk-Laufwerke mittels `Win` + `R` und:

  ~~~~~~
  RUNDLL32 SHELL32.DLL,SHHelpShortcuts_RunDLL Connect
  ~~~~~~

* Wählen Sie einen zu verwendenden Laufwerkbuchstaben
* Als Pfad für den Ordner verwenden Sie: `\\jumpstart-vm\vagrant-home`
* Wählen Sie zudem: `Verbindung bei Anmeldung wiederherstellen` und `Verbindung mit anderen Anmeldeinformationen herstellen`
* Im nächsten Dialog nutzen Sie als User/Passwort: `vagrant:vagrant` und wählen Sie `Anmeldedaten speichern`
