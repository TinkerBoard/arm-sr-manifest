# ARM System Ready Manifest for Tinker Board 2/2S

This is based on the following repository and modified for the Tinker Board 2/2S.

https://github.com/ankuraltran/ARMSRManifest

The information provided here provides the configuration and build setup for ARM System Ready compliance verification on Tinker Board series.

Please refer to the following URL to install Repo. 

https://source.android.com/setup/develop#installing-repo

Please refer to the following URL to understand how to download the AOSP-based source.

https://source.android.com/setup/build/downloading

To download the source for a product, please run the following commands.

    $ repo init -u https://github.com/TinkerBoard/arm-sr-manifest.git -b master -m NAME.xml
    $ repo sync

Here NAME.xml is the manifest file for the product. Regarding the manifest file for each product, please refer to the following table.

The following products are used for the verification.

|Product|Manifest|
|-|-|
|Tinker Board 2/2S|Tinker_Board_2.xml|

To get the toolkits, please run the following commands.

    $ cd build/
    $ make toolchains

To build the firmware, please run the following command.

    $ make

You can run the following commands to flash the firmware into the SD card for booting up from the SD card. Or you can use the software such as [balenaEtcher](https://www.balena.io/etcher/) to do the same thing as well.

    $ sudo dd if=./out/bin/u-boot-rockchip.bin of=/dev/sdx seek=64
    $ sync

To flash the firmware into the eMMC on the board of the product, you need to boot up the board into the UMS mode. Please have a SD card flashed with the official Tinker Board 2/2S images downloaded fron the following URL.Then, power on the board with this SD card insalled and the USB type C connected to a PC to boot the board into the UMS mode.

https://tinker-board.asus.com/download-list.html?product=tinker-board-2s

Once the board is booted into the UMS mode, uou can run the following commands to flash the firmware into the eMMC. Or you can use the software such as [balenaEtcher](https://www.balena.io/etcher/) to do the same thing as well.

    $ sudo dd if=./out/bin/u-boot-rockchip.bin of=/dev/sdx seek=64
    $ sync

To setup and access the debug console, please refer to the following URL for the detail infomration.

https://github.com/TinkerBoard/TinkerBoard/wiki/Developer-Guide#setting-up-a-serial-port-console-on-tinker-board-2s

The baudrate is 115200 and you should press 'e' to edit the commands before booting when see the graphical GRUB menu to add the following to the kernel commnad line.

    earlycon=uart8250,mmio32,0xff1a0000 console=uart8250,mmio32,0xff1a0000

The following configuration is used to run ACS and OS landing reports.
* Firmware: u-boot-rockchip.bin flashed in the eMMC
* ACS: ir_acs_live_image-21.09.img flashed in the SD card
* Distribution OSs:
    * Feora-IoT-ostree-aarch64-37-20221118.0.iso flashed in USB Disk
    * ubuntu-22.04.1-live-server-arm64.iso flashed in USB Disk

The followin OS distros/versions were used in the OS sniff test.
* Fedora Linux 37.2.221118.0
* Ubuntu 22.04.1 LTS
