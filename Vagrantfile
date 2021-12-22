# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  config.vm.define "master" do |master|
    master.vm.provider "libvirt" do |libvirt|
        libvirt.memory = 1024
        libvirt.cpus = 1
    end

    master.vm.box = 'master'
    master.vm.hostname = 'salt'
    master.vm.network "private_network", type: "dhcp"

    master.vm.provision "shell", inline: <<-SHELL
        systemctl enable --now salt-master.service
    SHELL
  end

  [1, 2].each do |i|
    config.vm.define "minion#{i}" do |minion|
      minion.vm.provider "libvirt" do |libvirt|
        libvirt.memory = 1024
        libvirt.cpus = 1
      end

      minion.vm.box='minion'
      minion.vm.hostname="minion#{i}"
      minion.vm.network "private_network", type: "dhcp"

      minion.vm.provision "shell", inline: <<-SHELL
        systemctl enable --now salt-minion.service
      SHELL
    end
  end
end
