Erstellen des VM Images
=======================

Diese VM ist auf Desktop-Betrieb über die Virtualbox-Visualisierung ausgelegt
und erlaubt auf möglichst einfache Art die Verwendung der Entwicklungsumgebung.

Die Generierung ist fast vollständig automatisiert.
Der Komplette Vorgang wird folgend beschrieben.


Erzeugung mittels Vagrant und Ansible
-------------------------------------

Siehe: [](../headless-vm/README.md)

* generate with vagrant
* if it hangs:
  * login via GUI and connect both network connections
  * or manually do shutdown and `vagrant up --provision` 
  * or only `vagrant provision` 
* start up with GUI

If problems with DHCP arises:

~~~
VBoxManage list dhcpservers
// and delete old ones with:
VBoxManage dhcpserver remove --netname "HostInterfaceNetworking-vboxnet0"
~~~


Abschliessen
------------

* shutdown VM
* remove existing shared folders
* export as appliance OVA 2.0: `prcpp-desktop.ova`
* split up file into smaller ones and deploy to AD and USB sticks
