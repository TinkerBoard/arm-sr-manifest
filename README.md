# ARM System Ready Manifest for Tinker Board 2/2S

This is based on the following repository and modified for the Tinker Board 2/2S.

https://github.com/ankuraltran/ARMSRManifest

The information provided here provides the configuration and build setup for ARM System Ready compliance verification on Tinker Board series.

Please refer to the following URL to install Repo. 

https://source.android.com/setup/develop#installing-repo

Please refer to the following URL to understand how to download the AOSP-based source.

https://source.android.com/setup/build/downloading

To download the source for a product, please run the following commands.
```bash
repo init -u https://github.com/TinkerBoard/arm-sr-manifest.git -b master -m NAME.xml
repo sync
```
Here NAME.xml is the manifest file for the product. Regarding the manifest file for each product, please refer to the following table.

The following products are used for the verification.

|Product|Manifest|
|-|-|
|Tinker Board 2/2S|Tinker_Board_2.xml|

Please run the following commands get the toolkits first.
```bash
cd build/
make toolchains
```

Then, you can run the following command to build the firmware.
```bash
sudo make
```
You will have the firmware as out/bin/u-boot/tb2-firmware-efi.img.

If you want to build the capsule, you can run the following command.
```bash
sudo make capsule
```
You will have the capsule as out/bin/capsule/capsule.bin.

To flash the firmware into the SD card for booting up from the SD card, you can run the following commands. Or you can use the software such as [balenaEtcher](https://www.balena.io/etcher/) to do the same thing as well.
```bash
sudo dd if=./out/bin/u-boot/tb2-firmware-efi.img of=/dev/sdx
sync
```
Here `sdx` should be replaced with the device node to which you want to flash.

To flash the firmware into the eMMC on the board of the product, you need to boot up the board into the UMS mode. Please have a SD card flashed with the official Tinker Board 2/2S images downloaded from the following URL. Then, power on the board with this SD card installed and the USB type C connected to a PC to boot the board into the UMS mode.

https://tinker-board.asus.com/download-list.html?product=tinker-board-2s

Once the board is booted into the UMS mode, you can run the same commands above to flash the firmware into the eMMC.

To setup and access the debug console, please refer to the following URL for the detail information.

https://github.com/TinkerBoard/TinkerBoard/wiki/Developer-Guide#setting-up-a-serial-port-console-on-tinker-board-2s

The baudrate is 115200 and you should press 'e' to edit the commands before booting when see the graphical GRUB menu to add the following to the kernel command line.

    earlycon=uart8250,mmio32,0xff1a0000 console=uart8250,mmio32,0xff1a0000

The following configuration is used to run ACS and OS landing reports.
## Tinker Board 2S
* Firmware: tb2-firmware-efi.img flashed in the eMMC
* ACS: ir_acs_live_image-21.09.img flashed in the SD card
* Distribution OSs:
    * Feora-IoT-ostree-aarch64-37-20221118.0.iso flashed in the USB drive
    * ubuntu-22.04.1-live-server-arm64.iso flashed in the USB drive

## Tinker Board 2
* Firmware: tb2-firmware-efi.img flashed in the SD card
* ACS: ir_acs_live_image-21.09.img flashed in the USB drive
* Distribution OSs:
    * Feora-IoT-ostree-aarch64-37-20221118.0.iso flashed in the USB drive
    * ubuntu-22.04.1-live-server-arm64.iso flashed in the USB drive

The following OS distros/versions were used in the OS sniff test.
* Fedora Linux 37.2.221118.0
* Ubuntu 22.04.1 LTS

To run ACS or Distribution OS, we will need to change the boot_targets in u-boot shell based on the media we want to boot from. The default boot_targets is `mmc0 mmc1 usb0 pxe dhcp sf0`.
## To change to the SD card:
```bash
setenv boot_targets mmc1
saveenv
boot
 ```
## To change to the USB drive
```bash
usb start
setenv boot_targets usb0
saveenv
boot
```
