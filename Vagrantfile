# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "ubuntu/trusty64"
  # config.vm.box = "Ubuntu 14.04"

  # Uncomment if you want to download the last release.
  # config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/trusty-server-cloudimg-amd64-juju-vagrant-disk1.box"

  $script = <<SCRIPT
    apt-add-repository -y ppa:juju/stable
    apt-get update
    apt-get install -y juju-local
    sudo -H -u vagrant bash -c 'juju generate-config; juju switch local; juju bootstrap 2> /dev/null'
SCRIPT
  config.vm.provision "shell", inline: $script

  ###########################################################
  # Configure routing so we can connect to lxc w/o sshuttle #
  # $ vagrant plugin install vagrant-triggers               #
  ###########################################################
  config.trigger.after [:provision, :up, :reload] do
    system('sudo route add -net 10.0.3.0/24 10.0.4.2 >/dev/null')
  end
 
  config.trigger.after [:halt, :destroy, :suspend] do
    system('sudo route delete -net 10.0.3.0/24 10.0.4.2 >/dev/null')
  end

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "10.0.4.2"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  # config.ssh.forward_agent = true

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  config.vm.provider "virtualbox" do |vb|
    # Don't boot with headless mode
    # vb.gui = true
  
    # Use VBoxManage to customize the VM. For example to change memory:
    vb.customize ["modifyvm", :id, "--memory", "1024"]

    # vb.customize ["modifyvm", :id, "--usb", "on"]
     #vb.customize ["usbfilter", "add", "0", "--target", :id, "1197123b", "--vendorid", "0x04e8"]
    # vb.customize ["usbfilter", "add", "0", "--target", :id, "--name", "android", "--vendorid", "0x18d1"]

    # vb.customize ['modifyvm', :id, '--nicpromisc1', 'allow-all']
  end

end
