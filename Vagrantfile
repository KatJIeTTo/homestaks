# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
   config.vm.box = "debian10"

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
   config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
   config.vm.network "private_network", ip: "192.168.10.11"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder  ".", "/vagrant", disabled: false

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
   config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
     vb.name = "lxcdeb"
     vb.memory = "512"
   end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
   config.vm.provision "shell", inline: <<-SHELL
     apt-get update
     apt purge gnupg
     apt install --reinstall gnupg2
     apt install dirmngr
     apt-get install -y mc
     apt-get install -y lxc-templates
     apt-get install -y lxc
     apt-get install -y bridge-utils
     mkdir -p /home/vagrant/.config/lxc
     sudo echo "lxc.id_map = u 0 100000 65536" >> /home/vagrant/.config/lxc/root.conf
     sudo echo "lxc.id_map = g 0 100000 65536" >> /home/vagrant/.config/lxc/root.conf
     sudo echo "lxc.network.type = veth" >> /home/vagrant/.config/lxc/root.conf
     sudo echo "lxc.network.link = lxcbr0" >> /home/vagrant/.config/lxc/root.conf
     sudo echo "lxc.network.flags = up" >> /home/vagrant/.config/lxc/root.conf
     sudo echo kernel.unprivileged_userns_clone=1 >> /etc/sysctl.conf
     sudo echo "vagrant veth lxcbr0 10" >> /etc/lxc/lxc-usernet
     sudo echo "USE_LXC_BRIDGE="true"
     LXC_BRIDGE="lxcbr0"
     LXC_ADDR="10.0.3.1"
     LXC_NETMASK="255.255.255.0"
     LXC_NETWORK="10.0.3.0/24"
     LXC_DHCP_RANGE="10.0.3.2,10.0.3.254"
     LXC_DHCP_MAX="253"
     LXC_DHCP_CONFILE=""
     LXC_DOMAIN=""" > /etc/default/lxc-net
     systemctl enable lxc-net
     systemctl start lxc-net
     sudo lxc-create -n c1 -f /home/vagrant/.config/lxc/root.conf -t download -- --dist centos --release 8-Stream --arch amd64 --keyserver "hkp://keyserver.ubuntu.com"
     sudo lxc-create -n c2 -f /home/vagrant/.config/lxc/root.conf -t download -- --dist centos --release 8-Stream --arch amd64 --keyserver "hkp://keyserver.ubuntu.com"
   SHELL
end
