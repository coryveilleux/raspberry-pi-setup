# Raspberry Pi Setup

## Download and Etch to Micro SD Card

1.  Download the latest _Raspbian with desktop_ from
    https://www.raspberrypi.org/downloads/raspbian/:
    https://downloads.raspberrypi.org/raspbian_latest

2.  Download [Etcher](https://www.balena.io/etcher/)

3.  Etch Raspbian zip to your micro SD card with Etcher.
    In settings, uncheck _Auto-unmount on success_.

    ![](./images/etcher-settings.png)

4.  Enable ssh for headless setup:

    ```bash
    cd /Volumes/boot
    touch ssh
    ```

## Start Up Raspberry Pi

1.  Plug raspberry pi into an ethernet port on your router and connect it to
    power.

2.  Use your router web interface or app to determine what IP address has been
    given to your raspberry pi.

## Initial Raspberry Pi Configuration

1.  SSH into your raspberry pi (default user is `pi` and default password is
    `raspberry`):

    ```bash
    ssh pi@[raspberry pi IP address]
    # enter raspberry as the password
    ```

2.  Perform initial configuration for your raspberry pi:

    ```bash
    sudo raspi-config
    ```

    i. Change your password - TODO: details

    ii. TODO
