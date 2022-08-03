# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
   config.vm.box = "debian10"
   config.vm.network "forwarded_port", guest: 80, host: 8080
   config.vm.network "forwarded_port", guest: 81, host: 8081
   config.vm.network "private_network", ip: "192.168.10.10"
 
   config.vm.provider "virtualbox" do |vb|
     vb.name = "debian10dev"
     vb.memory = "512"
   end
 
   config.vm.provision "shell", inline: <<-SHELL
     apt-get install -y mc
     apt-get install -y apache2
     apt-get install -y php7.3
   SHELL
 
   config.vm.provision "file", run: "always", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/id_rsa.pub"
   config.vm.provision "file", run: "always", source: "~/.ssh/id_rsa", destination: "~/.ssh/id_rsa"
 end