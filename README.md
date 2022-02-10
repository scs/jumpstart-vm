jumpstart-vm
============

[![vagrant-up](https://github.com/scs/jumpstart-vm/actions/workflows/vagrant-up.yml/badge.svg)](https://github.com/scs/jumpstart-vm/actions/workflows/vagrant-up.yml)

Für den [Jumpstart-Kurs](https://github.com/scs/jumpstart-docs) wird eine Entwicklung-Umgebung benötigt,
in welcher alle Tools für die entsprechenden Übungs-Aufgaben installiert sind.
Damit diese Umgebung nicht mit anderen bereits installierten Tools kollidiert
und für alle Teilnehmen konsistent ist,
wird dafür eine virtuelle Machine eingerichtet.

Als Basis wird Ubuntu-Desktop verwendet,
auf welcher mittels `ansible` im Folgenden alle benötigten Tools automatisiert installiert werden.


Rechen-Performance der VM
-------------------------

Damit die VM beinahe native Rechen-Leistung erreicht,
sollte sicherstellt werden,
dass der Rechner alle benötigten Virtualisierung-Technologien aktiviert hat.
Diese finden sich in den BIOS/UEFI-Einstellungen.

Unter Intel-CPUs: `VT-x` und `VT-d`

Unter AMD-CPUs: `AMD-V` und `AMD-Vi` oder `SVM`



Installation
============

Es gibt mehrere Möglichkeiten wie die VM aufgesetzt werden kann:

* Virtualbox:
  * Funktioniert am besten, wenn kein anderer Hypervisor (VMWare, Hyper-V, Docker-Desktop) im System aktiv ist.
  * Sehr flexibel was die Verwendung des virtuellen Desktops betrifft.
  * [Installation mit Virtualbox](docs/virtualbox.md)
* Hyper-V:
  * Beste Kompatibilität, wenn auch Docker-Desktop oder WSL2 auf dem physischen PC laufen muss.
  * Bedienung über den virtuellen Desktop eingeschränkt.
    Hier wird die Verwendung mittels SSH und X-Forwarding empfohlen.
  * [Installation mit Hyper-V](docs/hyperv.md)


Verwendung der Desktop-VM
=========================

Die VM kann direkt im Virtualbox/Hyper-V-Manager GUI gestartet werden.
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


Headless-Verwendung
-------------------

Die VM ist auch für die Benutzung ohne Desktop-Oberflache und Virtualbox-GUI ausgelegt.
Als Alternative kann mittels SSH und X11-Forwarding gearbeitet werden.
Weitere Infos dazu unter: [docs/headless_and_vagrant.md](docs/headless_and_vagrant.md)
