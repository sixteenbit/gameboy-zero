# Gameboy Zero (WIP)

## Initial setup

First, connect to the pi using ssh and run the following:

```bash
sudo raspi-config
```

* 1 > Change password
* 2 > Change hostname
* 4 > Setup localization
* 7 > Advanced Options > Expand Filesystem
* Reboot

Next, change the font for the console to be legible for a smaller screen.

```bash
sudo dpkg-reconfigure console-setup
```

Select UTF-8 > Guess optimal character set > Terminus > 12x24 (framebuffer only)

## GPIO Setup

```bash
cd; curl -O https://raw.githubusercontent.com/adafruit/Raspberry-Pi-Installer-Scripts/master/retrogame.sh; sudo bash retrogame.sh
```

When you run the retrogame.sh file, you will be presented with a menu.

Select the PiGRRL 2 Controls option, and the program will continue to install some modules.

Once complete, reboot.