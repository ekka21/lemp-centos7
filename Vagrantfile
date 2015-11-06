# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

    config.vm.box = "puppetlabs/centos-7.0-64-puppet"

    config.vm.network "private_network", ip: "192.168.33.90"

    config.vm.network "forwarded_port", guest: 80, host: 8080

    config.vm.network "forwarded_port", guest: 3306, host: 33060

    config.vm.synced_folder ".", "/vagrant", disabled: true

end
