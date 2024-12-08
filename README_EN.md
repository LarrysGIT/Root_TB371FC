# Root_TB371FC
Lenovo XiaoXin Pad Pro 2023 12.7 (2023) root and 9008 rescue

# Tools
## platform-tools
`https://developer.android.com/tools/releases/platform-tools`<br>
It contains adb fastboot

## Qualcomm Flash Image Loader (9008)
`https://qfiltool.com`<br>
Dont forget to install \Driver\Qualcomm USB Driver V1.0.exe

## root apk
`https://github.com/topjohnwu/Magisk/releases`

# ZUI ROM
Lenovo doesn't provide their ZUI ROMs, so far i managed to find 3 versions from XDA forum<br>
ZUI 15.0.405(suggest to download for 9008 rescue)<br>
`https://www.mediafire.com/file/vdfae5lei59frbo/TB371FC-ZUI_15.0.405-QFIL.zip/file`<br>

Other versions for record purpose,<br>
ZUI 15.0.154 Dev version<br>
`https://mirrors.lolinet.com/firmware/lenowow/Tab_P12_Pro_2023/TB371FC/`<br>
ZUI 15.0.440(this is the first version supports PC mode)<br>
`https://www.mediafire.com/file/k7pjpl2841rb8gl/TB371FC-ZUI_15.0.440_FASTBOOT_QFIL_v2.zip/file`<br>

2024 Sep 16 (proximately), 4pda user `VLAD S.` posted ZUI OTA ROMs<br>
`https://4pda.to/forum/index.php?showtopic=1086877&view=findpost&p=133146268`<br>
the following is instructions<br>
- The OTA roms are installed by the default ZUI system updater
- Rename the archive to OTA.zip and put it in the root of the disk
- Go to settings -> About tablet -> System update
- Tap 7 times on the version number, a hidden option for installing from the archive will open.
<br>
ROM `16.0.443`: `https://ota-cdn.lenovo.com/firmware/2024828185240998-6800.zip` (This ROM overall much smooth than 16.0.430)

# Can I root on current version of ZUI instead of downloading a previous release?
Unfortunately, no.<br>
In order to use root tool Magisk, it requires a matched version of boot.img from the current ROM.<br>
While Lenovo doesn't provide their ROMs, it is no way to obtain the correct boot.img.

# Enable developer mode
In ZUI, settings -> about -> software version (zui 15.0.xxx) tapping on it -> in developer mode<br>
Settings -> General -> Developer mode -> Enable USB debugging -> Connect to computer -> trust this computer

# Unlock bootloader
_You lost warranty if you unlock your device and there is no way to lock again, risk on your own_<br>

> If you're bootloader is already unlock skip this step.

Execute `adb reboot bootloader` to fastboot, you will see the tablet's serial number<br>
Go to Lenovo bootunloader unlock application website, provide your SN, Lenovo will send you a link via email<br>
`http://cdn.zui.lenovomm.com/developer/tabletboot/{your SN number}/sn.img`<br>
If you can't wait, you can also replace string `AAAAAAAA` with your SN number in the file `sn_AAAAAAAA.img` from this repo<br>
You need a hex editor to perform the SN update, notepad won't work<br>
Execute `fastboot flash unlock sn.img` to flash sn.img<br>
Execute `fastboot reboot` tablet is now showing unlocked

# 9008 flashing
_bootloader must be unlocked_ [#7](https://github.com/LarrysGIT/Root_TB371FC/issues/7)<br>
Open the device manager from Windows<br>
Disconnect your tablet from Windows<br>
Power off your tablet<br>
Open QFIL.exe to configure as following<br>

- configuration -> Firehose configuration<br>
- - Device type select "ufs"<br>
- - Validation mode select "0 - no validation"<br>
- - Reset after download check on<br>
- - OK<br>
- Build type select "Flat Build"<br>
- Programmer select `prog_firehose_ddr.elf` from your ROM version 405<br>
- Load XML _select ALL xml files_ (important) from your ROM 405<br>

Press and hold volume +, connect tablet with Windows via USB<br>
You should be able to see from the device manager, a device with suffix 9008 shows up<br>
Back to QFIL.exe click on `Download`, flash commences

# Install Magisk and root
Redo `Enable developer mode` and connect your tablet with Windows via USB<br>
Follow the Magisk instruction to patch boot.img and flash it.<br>
- https://topjohnwu.github.io/Magisk/install.html

My steps following<br>
Copy Magisk-v27.0.apk and boot.img (from ROM 405) to tablet<br>
Install Magisk apk and run the app<br>
From Magisk, install -> select and patch a file -> select boot.img<br>
Copy the patched magisk_patched_xxxxxxx.img to Windows<br>
Execute `adb reboot bootloader` to bootloader<br>
Execute `fastboot flash boot magisk_patched_xxxxxxx.img` to flash boot.img<br>
Enter ZUI and run Magisk, you will see the button `superuser` now is clickable<br>
root is completed

# Other versions of the boot.img
Thanks to the original uploader/poster<br>
 - https://4pda.to/forum/index.php?showtopic=1073502&st=1980#entry126890252

There are few other versions on XDA reposted from 4PDA (ZUI 15 boot.img 543/575/650)<br>
 - https://xdaforums.com/t/tb371fc-xiaoxin-pad-pro-2023-12-7-sharing-and-support.4642987/page-23#post-89442944
 - 15.0.543
 - 15.0.575
 - 15.0.650

Meanwhile, in the `boot.imgs` folder, you can find the following versions<br>
 - 15.0.664
 - 16.0.430
 - 16.0.443
 - 16.0.474

# Troubleshooting
## QFIL errors
If you have errors during the ROM flash with QFIL. For example Sahara protocol errors, or path not found errors try using QFIL installed from QPST `https://qpsttool.com/qpst-tool-v2-7-496` instead of the standalone version `https://qfiltool.com/qfil-tool-v2-0-3-5`.(https://github.com/LarrysGIT/Root_TB371FC/issues/1)<br>

## Update ZUI normally via OTA
If you had flahsed with a Magisk patched boot.img, you need to flash the original boot.img first, if you don't do such, OTA will loop between download and upgrade updates, and never finishes.<br>
Execte `adb reboot bootloader` to bootloader<br>
Execute `fastboot flash boot boot.img` to flash the original boot.img<br>

## Disable system OTA update
Execute `adb shell`<br>
Execute `su` it will give error `permission denied`<br>
In Magisk, allow shell to run as root<br>
Execute `su` again<br>
Execute `pm disable com.lenovo.ota`

## google apps are unable to connect to internet
ZUI install google framework already, however, it is disabled by default and version is behind<br>
Enable google framework from settings -> apps<br>
ZUI bases on Android 13, download the apk and install it, google services will work again<br>
`https://www.apkmirror.com/apk/google-inc/google-services-framework/google-services-framework-13-release/`
