WSL 2
=======

WSL 2 bietet die Möglichkeit, einen Linux-Kernel parallel zu Windows auszuführen.
Der Zugriff auf das Windows-Dateisystem ist möglich, jedoch etwas langsam.
Deshalb sollte man seine Dateien im WSL 2 Filesystem bearbeiten.
Mit den neuesten Versionen von Windows 10 und Windows 11 können auch
GUI-Anwendungen ausgeführt werden.
Es wird empfohlen, eine separate Ubuntu WSL 2 Instanz für den Jumpstart zu erstellen,
um mögliche Probleme mit anderen Projekten zu vermeiden,
falls du WSL2 bereits verwendest.

!!! Achtung !!!
------------

Falls in einem Projekt schon eine VirtualBox VM verwendet wird,
kann dies zu Problemen führen.
In diesem Fall die Variante [VirtualBox](./virtualbox.md)
verwenden.


Installation
------------

1. WSL 2 über Windows Store installieren: [WSL 2 in Windows Store](https://www.microsoft.com/store/productId/9P9TQF7MRM4R)
2. \
  a. Falls noch keine WSL 2 Instanz da ist: Ubuntu über Windows Store installieren: [Ubuntu 22.04 in Windows Store](https://www.microsoft.com/store/productId/9PN20MSR04DW)\
  b. Sonst: Eine zusätzliche WSL 2 Instanz mit ubuntu 22 erstellen: <https://endjin.com/blog/2021/11/setting-up-multiple-wsl-distribution-instances>\
3. `git clone https://github.com/scs/jumpstart-vm.git`
4. `cd jumpstart-vm`
5. `./provision.sh`. Achtung: Dieser Schritt installiert sehr viele Pakete in der VM
  und dauert entsprechend sehr lange.
6. Sicherstellen, dass systemd aktiviert ist:
   ```bash
   cat << 'EOF' | sudo tee /etc/wsl.conf

   [boot]
   systemd=true
   EOF
   ```
