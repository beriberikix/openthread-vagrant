# -*- mode: ruby -*-
# vi: set ft=ruby :

# cribbed from https://github.com/adafruit/esp8266-micropython-vagrant
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"

  # Virtualbox VM configuration.
  config.vm.provider "virtualbox" do |v|
    # Enable USB.
    v.customize ["modifyvm", :id, "--usb", "on"]
    v.memory = 2048
    v.customize ["usbfilter", "add", "0", "--target", :id, "--name", "CC2650 LaunchPad", "--vendorid", "0x0451", "--productid", "0xbef3"]
  end

  # downloads and configuration dependencies
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    echo "Installing dependencies..."

    # quiets some stdin errors
    export DEBIAN_FRONTEND=noninteractive

    sudo apt-get install -y python-software-properties
    sudo add-apt-repository -y ppa:terry.guo/gcc-arm-embedded
    sudo apt-get update -qq

    # wpandtund runtime & build requirements
    sudo apt-get install -y build-essential git make autoconf automake dbus \
                            libtool gcc g++ gperf flex bison texinfo gawk \
                            ncurses-dev libexpat-dev python sed python-pip \
                            libreadline6-dev libreadline6 libdbus-1-dev libboost-dev
    sudo apt-get install -y --force-yes gcc-arm-none-eabi

    sudo pip install pexpect

    echo "Installing OpenThread & wpandtund..."
    mkdir -p ~/src

    # install wpantund
    cd ~/src
    echo "installing wpantund"
    git clone --recursive https://github.com/openthread/wpantund.git
    cd wpantund
    sudo git checkout full/master
    ./bootstrap.sh
    ./configure --sysconfdir=/etc
    Make
    sudo make install

    # install OpenThread
    cd ~/src
    git clone --recursive https://github.com/openthread/openthread.git
    cd openthread
    ./bootstrap

    echo "OpenThread and wpantund setup complete! Examples can be found in ~/src/openthread/examples"
  SHELL

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

end
