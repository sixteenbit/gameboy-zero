# Gameboy Zero (WIP)

## Pre-boot

1. Install [RetroPie](https://retropie.org.uk/download/) using [Etcher](https://etcher.io/) or [Apple Pi Baker](https://www.tweaking4all.com/software/macosx-software/macosx-apple-pi-baker/).
1. Add blank ssh file to root of disk.
1. Add wpa_supplicant.conf with wifi credentials to root of disk

    **Example**

    ```
    country=US
    update_config=1
    ctrl_interface=/var/run/wpa_supplicant

    network={
        ssid="YOUR SSID"
        psk="YOUR PASSWORD"
        scan_ssid=1
    }
    ```
1. Add the following to `config.txt` located in the root fo the disk.

    ```
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
1. Update `cmdline.txt` to the following:

    ```
    dwc_otg.lpm_enable=0 console=tty3 root=PARTUUID=3d24ca30-02 rootfstype=ext4 elevator=noop fsck.repair=yes rootwait loglevel=3 consoleblank=0 plymouth.enable=0
    ```
1. Eject the disk and insert into the handheld.

## Post-boot

1. Once the device boots, it will reboot once the file-system has been resized.
1. Connect to the device using ssh.

    ```bash
    ssh pi@retropie
    ```
1. Update localization

    ```bash
    sudo raspi-config
    ```
