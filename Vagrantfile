# -*- mode: ruby -*-
# vi: set ft=ruby :

box = "ubuntu/bionic64"

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = box

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 3000

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"
  # config.vm.network "private_network", type: "dhcp"

  config.vm.define :web1 do |web1|
    web1.vm.hostname = "mt-web1"
    # web1.vm.provision "web1", type: "shell", :path => "create_web_server.sh", :privileged => false
    web1.vm.network :forwarded_port, id: "ssh", guest: 22, host: 7222
    web1.vm.network "private_network", ip: "192.168.33.10"
    web1.vm.provider "virtualbox" do |web1_vb|
      web1_vb.name = "mt_web1"
      web1_vb.cpus = 1
      web1_vb.memory = "2048"
    end
  end

  config.vm.define :web2 do |web2|
    web2.vm.hostname = "mt-web2"
    # web2.vm.provision "web2", type: "shell", :path => "create_web_server.sh", :privileged => false
    web2.vm.network :forwarded_port, id: "ssh", guest: 22, host: 7322
    web2.vm.network "forwarded_port", guest: 1323, host: 1323, host_ip: "127.0.0.1"
    web2.vm.network "private_network", ip: "192.168.33.11"
    web2.vm.provider "virtualbox" do |web2_vb|
      web2_vb.name = "mt_web2"
      web2_vb.cpus = 1
      web2_vb.memory = "2048"
    end
  end

  config.vm.define :web3 do |web3|
    web3.vm.hostname = "mt-web3"
    # web3.vm.provision "web3", type: "shell", :path => "create_web_server.sh", :privileged => false
    web3.vm.network :forwarded_port, id: "ssh", guest: 22, host: 7422
    web3.vm.network "private_network", ip: "192.168.33.12"
    web3.vm.provider "virtualbox" do |web3_vb|
      web3_vb.name = "mt_web3"
      web3_vb.cpus = 1
      web3_vb.memory = "2048"
    end
  end

  config.vm.define :db do |db|
    db.vm.hostname = "mt-db"
    # db.vm.provision "db", type: "shell", :path => "create_db_server.sh", :privileged => false
    db.vm.network :forwarded_port, id: "ssh", guest: 22, host: 7022
    db.vm.network "forwarded_port", guest: 3306, host: 3306
    db.vm.network "private_network", ip: "192.168.33.13"
    db.vm.provider "virtualbox" do |db_vb|
      db_vb.name = "mt_db"
      db_vb.cpus = 1
      db_vb.memory = "2048"
    end
  end
  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.
  # config.vm.provider "virtualbox" do |vb|
  #   vb.memory = "2048"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL

  config.vm.provision "shell", inline: <<-SHELL
    set -e
    export DEBIAN_FRONTEND=noninteractive
    apt-get update
    apt-get install -y --no-install-recommends ansible git

    GITDIR="${HOME}/isucon10-qualify"
    rm -rf ${GITDIR}
#    git clone https://github.com/isucon/isucon10-qualify.git ${GITDIR}
    git clone https://github.com/er-k-aki/isucon10-qualify.git ${GITDIR}
    (
      cd ${GITDIR}/provisioning/ansible
      PYTHONUNBUFFERED=1 ANSIBLE_FORCE_COLOR=true ansible-playbook -i allinone, --connection=local allinone.yaml
    )
    rm -rf ${GITDIR}
  SHELL
end
