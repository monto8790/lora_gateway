# The Things Network: iC880a-based gateway

Reference setup for [The Things Network](http://thethingsnetwork.org/) gateways based on the iC880a SPI concentrator with a Raspberry Pi host.

This installer targets the **SPI version** of the board, if you have the USB version, [check this branch](https://github.com/ttn-zh/ic880a-gateway/tree/master).

## Setup

Check [a step-by-step HOWTO in our wiki](https://github.com/ttn-zh/ic880a-gateway/wiki) for a detailed explanation of the hardware that needs to be purchased and the software setup you will have to configure. 

## Update

If you have a running gateway and want to update, simply run the installer again:

        $ cd ~/ic880a-gateway
        $ sudo ./install.sh spi
- Default password of a plain-vanilla RASPBIAN install for user `pi` is `raspberry`
- Use `raspi-config` utility to expand the filesystem (1 Expand filesystem):

        $ sudo raspi-config

- Reboot
- Configure locales and time zone:

        $ sudo dpkg-reconfigure locales
        $ sudo dpkg-reconfigure tzdata

- Make sure you have an updated installation and install `git`:

        $ sudo apt-get update
        $ sudo apt-get upgrade
        $ sudo apt-get install git

- Create new user for TTN and add it to sudoers

        $ sudo adduser ttn 
        $ sudo adduser ttn sudo

- To prevent the system asking root password regularly, add TTN user in sudoers file

        $ sudo visudo

Add the line `ttn ALL=(ALL) NOPASSWD: ALL`

:warning: Beware this allows a connected console with the ttn user to issue any commands on your system, without any password control. This step is completely optional and remains your decision.

- Logout and login as `ttn` and remove the default `pi` user

        $ sudo userdel -rf pi

- Configure the wifi credentials (check [here for additional details](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md))

        $ sudo nano /etc/wpa_supplicant/wpa_supplicant.conf 

And add the following block at the end of the file, replacing SSID and password to match your network:

                network={
                    ssid="The_SSID_of_your_wifi"
                    psk="Your_wifi_password"
                }
 
- Clone [the installer](https://github.com/ttn-zh/ic880a-gateway/) and start the installation

        $ git clone https://github.com/ttn-zh/ic880a-gateway.git ~/ic880a-gateway
        $ cd ~/ic880a-gateway
        $ sudo ./install.sh spi

- If you want to use the remote configuration option, please make sure you have created a JSON file named as your gateway EUI (e.g. `B827EBFFFE7B80CD.json`) in the [Gateway Remote Config repository](https://github.com/ttn-zh/gateway-remote-config). 
- **Big Success!** You should now have a running gateway in front of you!

# Credits

These scripts are largely based on the awesome work by [Ruud Vlaming](https://github.com/devlaam) on the [Lorank8 installer](https://github.com/Ideetron/Lorank).

