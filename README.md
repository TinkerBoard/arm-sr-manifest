# ARM System Ready Manifest for Tinker Board 2/2S

This is based on the following repository and modified for the Tinker Board 2/2S.

https://github.com/ankuraltran/ARMSRManifest

The information provided here provides the configuration and build setup for ARM System Ready compliance verification on Tinker Board series.

The following products are used for the verification

|Product|Manifest|
|-|-|
|Tinker Board 2/2S|Tinker_Board_2.xml|

Please refer to the following URL to install Repo. 

    https://source.android.com/setup/develop#installing-repo

Please refer to the following URL to understand how to download the AOSP-based source.

    https://source.android.com/setup/build/downloading

To check out the specific release:

    $ repo init -u https://github.com/TinkerBoard/arm-sr-manifest.git -b master -m NAME.xml

Here NAME.xml is the initial manifest file. Regarding the manifest file for each project, please refer to the above table.

To download the source tree to your working directory from the repositories as specified in the default manifest, please execute the following command.

    $ repo sync

To get the toolkits, please execute the following commands.

    $ cd build/
    $ make toolchains

To build the firmware, please execute the following command.

    $ make

To flash the firmware to SD card, execute the following command
sudo dd if=./out/bin/u-boot-rockchip.bin of=/dev/sdx seek=64
<New_Dir>/out/bin/u-boot$ sync

To flash the firmware to eMMC
1.	Please create a SD card with Tinker Board 2/2S image in 
https://tinker-board.asus.com/download-list.html?product=tinker-board-2s
2.	Power on the device with USB-C and SD card for UMS mode
3.	Flash Firmware to the UMS disk corresponded to eMMC

sudo dd if=./out/bin/u-boot-rockchip.bin of=/dev/sdx seek=64
<New_Dir>/out/bin/u-boot$ sync

