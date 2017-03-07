# OpenThread & wpantund build and test environment

Vagrant setup for using, testing & developing OpenThread and wpantund. Heavily inspired by [adafruit/atmel-samd-micropython-vagrant](https://github.com/adafruit/atmel-samd-micropython-vagrant). This is not an official Google product.

## Installation & Usage

1.  Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads).

2.  Install [Vagrant](https://www.vagrantup.com/downloads.html).

3.  Clone or download this repository to a directory.

4.  In a terminal navigate to the directory and provision the VM by running:

        vagrant up

    The very first time this command is run it will take some time (~30 minutes)
    to download the operating system and setup the OpenThread toolchain and
    install wpandtund.

5.  After the VM is provisioned and up use the `vagrant ssh` command to enter
    the VM.  Once inside the VM the latest OpenThread and wpantund source will
    be in the `~/src/openthread` directory.  You can enter this folder
    and compile the OpenThread by running:

        cd ~/src/openthread
        make -f examples/Makefile-posix

6.  You can exit the VM at any time with the `exit` command.

    **Note even after exiting the VM will continue to run in the background!**
    Use the `vagrant halt` command to stop the VM.

    Once halted to enter the VM again use the `vagrant up` and then `vagrant ssh`
    commands again.

See more [details on Vagrant usage here](https://www.vagrantup.com/docs/getting-started/).
