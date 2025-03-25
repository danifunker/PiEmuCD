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
-   Copy the `piemucd.py` to the home folder of your user
-   Add `sudo python3 ~/piemucd.py` to the end of `~/.profile` to have the script start up on login
-   Set up the user to automatically login on startup using `sudo raspi-config`
-   Install flask `sudo apt-get install python3-flask`

3. Main interface

-   When PiEmuCD is run, it automatically starts up in CD emulation mode. If no `to-be-mounted.txt` file is present on the root of the image store partition, the operation will fail.
-   PiEmuCD has it's own command interface with few commands supported. Type `help` to get a list of all available commands.
-   `switch` will switch modes, `switch cdrom` will switch to CD-ROM emulation mode and `switch store` will switch to the image store being mounted on the virtual USB drive.

4. "To be mounted" file

-   The `to-be-mounted.txt` file specified which of the images on the image store should be mounted in CD-ROM emulation mode.
-   A sample file is provided in the repository.
-   File is placed inside of `/opt/usbide` (the folder will need to be created by `sudo mkdir -p /opt/usbide` then `sudo chgrp -R 1000 /opt/usbode` and finally `sudo chmod 664 /opt/usbode/*.txt` )

## Llama-ITX Notes
1. This works best with only a single ISO file being loaded
2. If booting from scratch, on my Pi Zero 2 W it takes about 45 seconds to boot up into the ISO, so if you are cold booting the Llama and want to boot from disk, wait a bit in the BIOS screen. 
3. Only a single USB is required to be connected to the Pi Zero 2 W for this application (so far) it CAN work with the data-only connection (USB Port closer to the HDMI/MicroSD slots)
4. When operating in storage mode, be reminded this is an interface via ExFAT, so it will not be possible to access the filesystem on Operating Systems priror to Windows XP with the hotfix installed.


## Todo
Since finding this project, I have the following todos:
- Mount Bin/Cue files to support CDDA 
- Figure out a way to copy ISOs over the network
- Create a web interface to manage which disc image is loaded

## Strech goal:
Maybe create a method to change the ISO through a DOS program or TSR (I have no experience with this though)

Feel free to contribue to the project.