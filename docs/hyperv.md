Hyper-V
=======

Sollte Hyper-V selber oder in Kombination mit Docker-Desktop oder WSL2 bereits auf dem System installiert sein,
bietet Hyper-V die bessere Performance als [Virtualbox](./virtualbox.md).
Allerdings sollte hier dann mittels [SSH und X-Forwarding](./headless.md) gearbeitet werden.


Installation
------------

Hyper-V ist ein Window-Feature dass nur noch aktiviert werden muss:
[enable-hyper-v](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)

Nach der Aktivierung von Hyper-V kann eine neue Linux-VM erstellt
und mit der Installation von Ubuntu begonnen werden.

* Genau dieses LTS Image von Ubuntu Desktop herunterladen: [ubuntu-desktop-22.04](https://ubuntu.com/download/desktop)
* In Hyper-V-Manager eine neue VM erstellen
  * Name: jumpstart-vm
  * Typ: Generation 2
  * Arbeitsspeicher beim Start: 2048
  * Netzwerk Verbindung: Default Switch
  * Festplatten Grösse: 64 GB
  * Betriebssystem von ISO installieren (hier das heruntergeladene Ubuntu ISO auswählen)
  * Fertigstellen
* VM konfigurieren: Einstellungen
  * Sicherheit -> Sicherer Start: Deaktivieren
  * Hardware -> Prozessor: min. 2 virtuelle Prozessoren
  * Hardware -> Arbeitsspeicher -> Maximaler RAM: 4096 (oder mehr)
* VM starten
  * Wenn Fenster nicht automatisch startet: "Verbinden"
  * "Install Ubuntu"
  * "Minimal Installation"
  * "Erase Disk and install Ubuntu"
  * Computers name: jumpstart-vm
  * Username: vagrant
  * Password vagrant
* VM neu starten (Boot-Medium wird automatisch entfernt)
* Tools installieren:
  * Konsole öffnen (`Terminal`)
  * `sudo apt update`
  * `sudo apt install -y git openssh-server`
  * Ab jetzt kann auch [headless](./headless.md) gearbeitet werden.
  * `git clone https://github.com/scs/jumpstart-vm.git`
  * `cd jumpstart-vm`
  * `./provision.sh`. Achtung: Dieser Schritt installiert sehr viele Pakete in der VM
    und dauert entsprechend sehr lange.
  * Abhängig von der Internetverbindung kann das mehr als 30 Minuten dauern.
  * Falls der Schritt fehlschlägt,
    zuerst mit `sudo apt update && sudo apt upgrade -y` alle existierenden Pakete updaten
    und dann nochmals mit `./provision.sh` versuchen.
  * Wenn alles gut läuft müsste am Schluss eine ähnliche Zeile erscheinen die zwingend `failed=0` anzeigt:
    ~~~~~~
    PLAY RECAP ************************************************************************************************
    127.0.0.1           : ok=144  changed=29   unreachable=0    failed=0    skipped=32   rescued=0    ignored=0
    ~~~~~~
* VM neu starten`


VM Tuning (optional)
---------

Bei Bedarf können Teilaspekte der VM auch nachträglich über das Hyper-V-Manager GUI angepasst werden.
Dazu muss die VM gestoppt sein.

* Min/Max Arbeitsspeicher
* CPUs hinzufügen


Austauschen von Dateien zwischen VM und Host
--------------------------------------------


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
