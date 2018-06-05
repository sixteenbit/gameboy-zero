# Sixteenbit RetroPie Image

[Installation](#Installation)
[Manually build the image](#Manually-build-the-image)

## Installation

1. Download the latest pre-made image (https://github.com/sixteenbit/gameboy-zero/releases)
1. Unzip so that you have an .img file
1. Use [Etcher](https://etcher.io/) or [Apple Pi Baker](https://www.tweaking4all.com/software/macosx-software/macosx-apple-pi-baker/) to copy img to SD card
1. Create a blank file named `ssh` on the root of the card which enables ssh
1. Create a file named `wpa_supplicant.conf` on the root of the card with your WiFi credentials. [Example](https://github.com/sixteenbit/gameboy-zero/blob/master/wpa_supplicant.conf)

## Manually build the image

### Flash RetroPie

1. Download RetroPie 4.3 https://github.com/RetroPie/RetroPie-Setup/releases/tag/4.3
1. Unzip so that you have an .img file
1. Use [Etcher](https://etcher.io/) or [Apple Pi Baker](https://www.tweaking4all.com/software/macosx-software/macosx-apple-pi-baker/) to copy img to SD card
1. Create a blank file named `ssh` on the root of the card which enables ssh
1. Create a file named `wpa_supplicant.conf` on the root of the card with your WiFi credentials. [Example](https://github.com/sixteenbit/gameboy-zero/blob/master/wpa_supplicant.conf)

### Initial setup

First, connect to the pi using ssh and run the following:

```bash
sudo raspi-config
```

- 2 > Change hostname
- 4 > Setup localization
- Reboot

Next, change the font for the console to be legible for a smaller screen.

```bash
sudo dpkg-reconfigure console-setup
```

Select UTF-8 > Guess optimal character set > Terminus > 12x24 (framebuffer only)

### Run updates

```bash
sudo apt-get update; sudo apt-get upgrade -y;
```

### Optimizations

First add the following to the end of `/boot/config.txt`

```bash
sudo nano /boot/config.txt
```

```bash
# Disable Pi Status LED
dtparam=act_led_trigger=none
dtparam=act_led_activelow=on

# Disable Bluetooth
# dtoverlay=pi3-disable-bt

# Disable Rainbow Splash
disable_splash=1

# Overclocking
arm_freq=1000
gpu_freq=500
core_freq=500
sdram_freq=500
sdram_schmoo=0x02000020
over_voltage=2
sdram_over_voltage=2

# Rotate display
display_rotate=2
```

Next, update `/boot/cmdline.txt` to the following:

1. Change `elevator=deadline` to `elevator=noop`
1. Change `console=tty1` to `console=tty3` to redirect boot messages to the third console.

### GPIO Setup

```bash
cd; curl -O https://raw.githubusercontent.com/adafruit/Raspberry-Pi-Installer-Scripts/master/retrogame.sh; sudo bash retrogame.sh
```

When you run the retrogame.sh file, you will be presented with a menu.

Select the PiGRRL 2 Controls option, and the program will continue to install some modules.

Once complete, reboot.

### Enable USB Audio

`sudo nano /etc/modprobe.d/alsa-base.conf`

```bash
options snd_usb_audio index=0
options snd_bcm2835 index=1
options snd slots=snd-usb-audio,snd-bcm2835
```

#### Fix for splashscreen audio

`sudo nano /etc/init.d/asplashscreen`

search for `omxplayer -o both -b --layer 10000 "$line"`

Change `both` to `alsa`

### Custom Splashscreen

[AJRedfern](http://www.sudomod.com/forum/viewtopic.php?f=42&t=1440) did an awesome job creating a splashscreen. I've included his latest (43v2-1) in this repo but be sure to check his thread for any updates.

First, connect to the pi and download the mp4 to the splashscreens directory/

```bash
cd ~/RetroPie/splashscreens
wget https://github.com/sixteenbit/gameboy-zero/raw/master/splashscreens/GBZ-Splash-Screen.mp4
```

Next, load the Retropie Setup script.

```bash
sudo bash ~/RetroPie-Setup/retropie_setup.sh
```

Next, go to:

1. Configurations / tools
1. splashscreen
1. Choose splashscreen
1. Own/Extra splashscreens (from /home/pi/RetroPie/splashscreens)

Finally, select GBZ-Splash-Screen.mp4 and perform a reboot.

### Install theme for 3.5" screens

[RxBrad](http://sudomod.com/forum/viewtopic.php?t=1549) has the best theme for a smaller screen in my opinion.

Installation is simple. Run the RetroPie Setup script again.

```bash
sudo ~/RetroPie-Setup/retropie_setup.sh
```

Next, go to:

1. Configurations / tools
1. esthemes

Next, scroll down to find "62 rxbrad/gbz35" and "63 rxbrad/gbz35-dark" hit Enter to install. Now the theme should show up within RetroPie UI Settings.

### Optional Overclocking

Add the following to the end of this `/boot/config.txt`

```bash
sudo nano /boot/config.txt
```

```bash
# Overclocking
arm_freq=1000
gpu_freq=500
core_freq=500
sdram_freq=500
sdram_schmoo=0x02000020
over_voltage=2
sdram_over_voltage=2
```

[Based on recommended overclocking found here.](https://retropie.org.uk/docs/Overclocking/#raspberry-pi-zero)
