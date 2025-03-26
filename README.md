# PiEmuCD

### Turn your Pi Zero into one or more virtual USB CD-ROM drives!

PiEmuCD is a Python script that uses the Linux USB Gadget kernel modules to turn your Raspberry Pi Zero (W) into one or more emulated USB CD-ROM drives.

**Documentation is a work in progress.**

## Installation

1. **Prepare the SD Card**

-   Flash Raspberry Pi OS Lite (Bullseye) to an SD Card (16 GB minimum recommended size)
    -   Use the Pi Imager tool to preconfigure hostname, login and locale
-   Use a partitioning tool to:
    -   Extend system partition to 4 GiB
    -   Create new partition, exFAT for the rest of the SD card -> this partition is called the **image store**.
-   Edit files from boot partition
    -   Add `dtoverlay=dwc2` to `config.txt`
    -   From `cmdline.txt` remove `quiet` and `init=/usr/lib/raspberrypi-sys-mods/firstboot` to prevent the OS from resizing the root partition on first boot

2. **Configure the Raspberry Pi**

-   Connect to the Pi, either via HDMI + keyboard, SSH or Serial
-   Create a folder for the USBODE `sudo mkdir -p /opt/usbode`
-   Copy the `piemucd.py` to the folder
-   Install flask `sudo apt-get install python3-flask`
-   Add the following file to (will need sudo access)`/lib/systemd/system/usbode.service`
```
[Unit]
Description=USBODE
After=multi-user.target
Conflicts=getty@tty1.service

[Service]
Type=simple
ExecStart=/usr/bin/python3 /opt/usbode/piemucd.py
StandardInput=tty-force

[Install]
WantedBy=multi-user.target
```
-   Reload the systemd service `sudo systemctl daemon-reload`
-   Start the service  `sudo systemctl start usbode.service`
-   Enable the service to start automatically on boot `sudo systemctl enable usbode.service`
-   To check the status of the service type `sudo systemctl status usbode.service`
-   To stop the service type `sudo systemctl stop usbode.service`
-   If you need to debug everything entirely make sure the service is stopped, then navigate to `/opt/usbode` and execute the script with `sudo python3 piemucd.py`. Once you are done debugging, type exit at the shell and that will gracefully close off the script.

3. Main interface

-  The USBODE interface will take about 30 seconds to startup, once configured.
-  For Initial setup, follow instructions at `http://<IPAddress>/setup`
-  Once the first image is mounted, everything is controlled via web, navigate to `http://<IPAddress>` or http://<name from preconfigured hostname in step 1>

## Llama-ITX Notes
1. This works best with only a single ISO file being loaded
2. If booting from scratch, on my Pi Zero 2 W it takes about 45 seconds to boot up into the ISO, so if you are cold booting the Llama and want to boot from disk, wait a bit in the BIOS screen. 
3. Only a single USB is required to be connected to the Pi Zero 2 W for this application (so far) it CAN work with the data-only connection (USB Port closer to the HDMI/MicroSD slots)
4. When operating in storage mode, be reminded this is an interface via ExFAT, so it will not be possible to access the filesystem on Operating Systems priror to Windows XP with the hotfix installed.


## Todo
Since finding this project, I have the following todos:
- Mount Bin/Cue files to support CDDA 
- Figure out a way to copy ISOs over the network

## Strech goal:
Maybe create a method to change the ISO through a DOS program or TSR (I have no experience with this though)

Feel free to contribue to the project.