# Setup CalyxOS without device installer

This guide shows how to install CalyxOS on a Google Pixel 4a (5g) without relying on the device installer provided by the CalyxOS team. Maybe you do not want to or can not execute the device-flasher on your system. Maybe you just want to go through all the steps yourself.  

The setup guide is created based on the procedure described in flasher.go ([Version 5cb204eb](https://gitlab.com/CalyxOS/device-flasher/-/blob/5cb204eb0712cd5887500ec8dfc3daf089ca0eac/flasher.go)). I assume not all steps are stricly necessary, but I just repeated the steps, which are taken by the device flasher. Make sure that only the android phone where you want to flash CalyxOS is connected. 

Although this guide mentions a specific device, the process should work for all devices, which support CalyxOS. Use at your own risk.

## 0. Create a folder

Not strictly needed, but I would suggest creating a folder to store both the platform tools and the factory image there.


## 1. Download factory image of device

Download the factory image of the device you want to flash from [https://calyxos.org/install/](https://calyxos.org/install/), verify digest and then unzip it into your created folder (no subfolders).

## 2. Download platform tools

Download the correct platform tools, which can be found by going to the ([gitlab code](https://gitlab.com/CalyxOS/device-flasher/-/blob/5cb204eb0712cd5887500ec8dfc3daf089ca0eac/flasher.go)) of the installer, find the function `getPlatformTools` and download the right version for your operation system from google. Once again, verify digest and unzip the downloaded file directly into your created folder (no subfolders).


## 3. Kill possbily running platform tools and start again

If you have adb installed on your system, make sure that adb is not running by running the command `adb kill-server`. Now use the adb executable, which you downloaded as part of the platform tools, to start the server by running `./adb start-server`.

## 4. Start flashing

Unlock your bootloader by running `./fastboot flashing unlock`.

After you have unlocked the device you should be able to flash a custom rom by executing all the steps outlined in the flash-all.sh file in sequence. If you flash CalyxOS onto a different device, change the img files used accordingly.

```
./fastboot flash bootloader bootloader-bramble-b5-0.4-8048800.img

./fastboot reboot-bootloader

./fastboot flash radio radio-bramble-g7250-00188-220211-B-8174514.img

./fastboot reboot-bootloader

./fastboot erase avb_custom_key

./fastboot flash avb_custom_key avb_custom_key.img

./fastboot --skip-reboot -w update image-bramble-sp2a.220505.002.zip

./fastboot reboot-bootloader
``` 

After all steps were successful, you can relock your bootloader by running `./fastboot flashing lock`.
Reboot your device by running `./fastboot reboot`.

