# -*- mode: ruby -*-
# vi: set ft=ruby :

vm_name = "jumpstart-vm"

Vagrant.configure("2") do |config|
  config.vm.box = "peru/ubuntu-20.04-desktop-amd64"
  config.vm.box_version = "20210317.01"

  config.vagrant.plugins = ["vagrant-disksize"]
  config.disksize.size = '64GB'

  config.vm.network "private_network", type: "dhcp"

  config.ssh.forward_agent = true
  config.ssh.forward_x11 = true

  config.vm.provider "virtualbox" do |vb|
    vb.name = vm_name
    vb.memory = "4096"
    vb.cpus = 2
    vb.customize ["modifyvm", :id, "--cpuexecutioncap", "90"]
    vb.customize ["modifyvm", :id, "--spec-ctrl", "on"]
    vb.customize ["modifyvm", :id, "--vram", "64"]
    vb.customize ["modifyvm", :id, "--graphicscontroller", "vmsvga"]
    vb.customize ["modifyvm", :id, "--clipboard-mode", "bidirectional"]
  end

  config.vm.hostname = vm_name

  config.vm.provision "shell",
    inline: "/vagrant/provision.sh",
    privileged: false

end
