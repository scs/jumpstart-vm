jumpstart-vm
============

[![ansible-lint](https://github.com/scs/jumpstart-vm/actions/workflows/ansible-lint.yml/badge.svg)](https://github.com/scs/jumpstart-vm/actions/workflows/ansible-lint.yml)

Für den [Jumpstart-Kurs](https://github.com/scs/jumpstart-docs) wird eine Entwicklung-Umgebung benötigt,
in welcher alle Tools für die entsprechenden Übungs-Aufgaben installiert sind.
Damit diese Umgebung nicht mit anderen bereits installierten Tools kollidiert
und für alle Teilnehmen konsistent ist,
wird dafür ein virtualisiertes Ubuntu eingerichtet.

Als Basis dafür wird Ubuntu 22.04 in WSL2 verwendet,
auf welcher mittels `ansible` im Folgenden alle benötigten Tools automatisiert installiert werden.

Es ist wichtig zu wissen, dass, sobald WSL2 installiert ist, Windows selber als VM in Hyper-V läuft.
WSL2 läuft dann in einer zusätzlichen VM neben Windows.
Und alle Distributionen (z.B. Ubuntu 22.04) laufen in dieser einen WSL2 VM.
Um Konflikte mit anderen Projekten zu vermeiden,
können aber auch mehrere Distros vom selben Typ in WSL2 installiert werden.


Rechen-Performance von WSL2
-------------------------

Damit VMs unter Hyper-V (und auch WSL2) beinahe native Rechen-Leistung erreichen,
sollte sicherstellt werden,
dass der Rechner alle benötigten Virtualisierung-Technologien aktiviert hat.
Diese finden sich in den BIOS/UEFI-Einstellungen.

Unter Intel-CPUs: `VT-x` und `VT-d`

Unter AMD-CPUs: `AMD-V` und `AMD-Vi` oder `SVM`


Erstellen einer separaten WSL Instanz
-------------------------------------

Als Erstes muss in einem Windows-Terminal die Basis-Distribution installiert werden.

~~~~~~
wsl --install -d Ubuntu-22.04
~~~~~~

Wenn WSL2 vorher nicht installiert/aktiviert war,
wird nach diesem ersten Versuch ein Reboot gemacht.
Danach muss der Befehl erneut ausgeführt werden,
damit die Distro installiert wird.
WSL wird nach dem Nutzernamen und Passwort für die neue Distro fragen.

Nun sind wir in der Linux-Shell und geben folgende Befehle ein:

~~~~~~
sudo apt update
sudo apt upgrade -y
sudo apt autoremove -y
exit
~~~~~~

Das hat nun die Distro auf den neusten Stand gebracht und die Linux-Shell beendet.
Wir sind nun wieder im Windows-Terminal und exportieren diese Distro als Basis-Image:

~~~~~~
cd $HOME
mkdir -p wsl
wsl --shutdown
wsl --export Ubuntu-22.04 wsl\ubuntu2204.tar.gz
~~~~~~

Wir haben nun ein "Backup"-Image der originalen Ubuntu 22.04 WSL Distro erstellt,
welches wir beliebig kopieren können.
Wir erstellen nun unsere "Jumpstart" Distro auf basis dieses Ubuntu 22.04 Images
und definieren den Default-User
und eine sinnvolle Maximum-Dateisystem-Grösse.

~~~~~~
wsl --import jumpstart-wsl wsl\jumpstart-wsl wsl\ubuntu2204.tar.gz
wsl --manage jumpstart-wsl --set-default-user <your_user>
wsl --shutdown
wsl --manage jumpstart-wsl --resize 128GB
~~~~~~


Installieren aller Tools in der Distro mittels Ansible
------------------------------------------------------

Installiere und starte [MobaXterm](https://mobaxterm.mobatek.net/).
Das könnte man auch mit [Chocolatey](https://chocolatey.org/install) machen:
`choco install -y mobaxterm`.

MobaXterm wird alle WSL2 Distros detektieren.
Es verwendet auch automatisch alle SSH-Keys von einem laufendem `pageant`
oder kann seine eigenen Keys managen.

Wenn wir nun zu unserer `jumpstart-wsl` Distro verbunden, sind können wir dieses Repo klonen:

~~~~~~
cd
git clone git@github.com:scs/jumpstart-vm.git
~~~~~~

Falls du mit dieser Entwicklungsumgebung richtig arbeiten willst,
solltest du mindestens den Git User und E-Mail
im [playbook.yml](ansible/playbook.yml) in der `git-tools` Role anpassen.

Nun können wir das Provisioning mittels Ansible starten:

~~~
cd jumpstart-vm/
chmod +x provision.sh
./provision.sh
~~~

Danach muss die WSL Instanz neu gestartet werden.
Z.B: über das Windows-Terminal:

~~~~~~
wsl --terminate jumpstart-wsl
~~~~~~


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

* `code` (Visual Studio Code)
* `git gui` (git Commit Helper)
* `gitk` (git Tree Viewer)
* `valgrind` (Memory Analyzer)
* `nano` (Console Editor)
* `tmux` (Shell Multiplexer)
* `okular` (PDF Viewer)
* `fdfind` (fast search tool)
