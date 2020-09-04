# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config| #https://github.com/patrickdlee/vagrant-examples
    config.vm.hostname = ".gitlab.export"

    # The most common configuration options are documented and commented below.
    # For a complete reference, please see the online documentation at
    # https://docs.vagrantup.com.

    # Every Vagrant development environment requires a box. You can search for
    # boxes at https://vagrantcloud.com/search.
    #config.vm.box = "peru/ubuntu-18.04-desktop-amd64" # to use ubuntu desktop https://app.vagrantup.com/peru/boxes/ubuntu-18.04-desktop-amd64
    config.vm.box = "hashicorp/bionic64"

    # Disable automatic box update checking. If you disable this, then
    # boxes will only be checked for updates when the user runs
    # `vagrant box outdated`. This is not recommended.
    # config.vm.box_check_update = false

    # Create a public network, which generally matched to bridged network.
    # Bridged networks make the machine appear as another physical device on
    # your network.
    # config.vm.network "public_network"

    config.vm.provider :virtualbox do |virtualbox, override|
        #   # Display the VirtualBox GUI when booting the machine
        #   vb.gui = true
        #
        #   # Customize the amount of memory on the VM:
        #   vb.memory = "1024"
        virtualbox.customize ["modifyvm", :id, "--cpus", 2]
        virtualbox.customize ["modifyvm", :id, "--audiocontroller", "hda"]
        virtualbox.customize ["modifyvm", :id, "--memory", 2048]
        virtualbox.customize ["modifyvm", :id, "--accelerate2dvideo", "on"]
        virtualbox.customize ["modifyvm", :id, "--vram", 128]
        virtualbox.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
        #vb.customize ["modifyvm", :id, "--draganddrop", "bidirectional"]
        # virtualbox.customize ["modifyvm", :id, "--cableconnected1", "on"] # uncomment this line if a timeout appears in virtualbox boot
        virtualbox.gui = false
    end

    # shared same volume to share docker swarm key
    config.vm.define "machine_main_manager.gitlab.export" do |machine|
        machine.vm.hostname = "manager.main.gitlab.export"
        # Create a private network, which allows host-only access to the machine
        # using a specific IP.
        machine.vm.network "private_network", ip: "192.168.100.3"

        # Share an additional folder to the guest VM. The first argument is
        # the path on the host to the actual folder. The second argument is
        # the path on the guest to mount the folder. And the optional third
        # argument is a set of non-required options.
        # config.vm.synced_folder "../data", "/vagrant_data"
        machine.vm.synced_folder ".", "/vagrant/", owner: "vagrant", group: "vagrant", created: true

        machine.vm.provision :shell, inline: <<-SHELL
            sudo loadkeys fr # change keyboard to french keyboard (azerty)
            sudo apt-get update
            sudo apt-get install -y python3 python3-pip
            sudo pip3 install requests pyyaml=="5.1"
        SHELL
    end
end
  