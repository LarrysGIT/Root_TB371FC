# Root_TB371FC
Lenovo XiaoXin Pad Pro 2023 12.7 root and 9008 rescue

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
ZUI 15.0.154 Dev version(no need to download, just for reference)<br>
`https://mirrors.lolinet.com/firmware/lenowow/Tab_P12_Pro_2023/TB371FC/`<br>
ZUI 15.0.405(suggest to download)<br>
`https://www.mediafire.com/file/vdfae5lei59frbo/TB371FC-ZUI_15.0.405-QFIL.zip/file`<br>
ZUI 15.0.440(suggest to download, this is the first version supports PC mode)<br>
`https://www.mediafire.com/file/k7pjpl2841rb8gl/TB371FC-ZUI_15.0.440_FASTBOOT_QFIL_v2.zip/file`<br>
My ROM flashing path: 405 -> 440<br>
You may go directly to 440, I dont know if it gonna work or what.

# Can I root on current version of ZUI instead of downloading a previous release?
Unfortunately, no.<br>
In order to use root tool Magisk, it requires a matched version of boot.img from the current ROM.<br>
While Lenovo doesn't provide their ROMs, it is no way to obtain the correct boot.img.

# Enable developer mode
In ZUI, settings -> about -> software version (zui 15.0.xxx) tapping on it -> in developer mode<br>
Settings -> General -> Developer mode -> Enable USB debugging -> Connect to computer -> trust this computer

# Unlock bootloader
*You lost warranty if you unlock your device and there is no way to lock again, risk on your own*<br>
Execute `adb reboot bootloader` to fastboot, you will see the tablet's serial number<br>
Go to Lenovo bootunloader unlock application website, provide your SN, Lenovo will send you a link via email<br>
`http://cdn.zui.lenovomm.com/developer/tabletboot/{your SN number}/sn.img`<br>
If you can't wait, you can also replace string `AAAAAAAA` with your SN number in the file `sn_AAAAAAAA.img` from this repo<br>
You need a hex editor to perform the SN update, notepad won't work<br>
Execute `fastboot flash unlock sn.img` to flash sn.img<br>
Execute `fastboot reboot` tablet is now showing unlocked

# 9008 flashing
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
- Load XML *select ALL xml files* (important) from your ROM 405<br>

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
Execute `adb reboot bootloader` to fastboot<br>
Execute `fastboot flash boot magisk_patched_xxxxxxx.img` to flash boot.img<br>
Enter ZUI and run Magisk, you will see the button `superuser` now is clickable<br>
root is completed

# Update from 405 -> 440
Update the system requires to do *Install Magisk and root* again<br>
Execte `adb reboot bootloader` to fastboot<br>
Execte `flash_all.bat` from ROM 440

# Disable system update
Execute `adb shell`<br>
Execute `su` it will give error `permission denied`<br>
In Magisk, allow shell to run as root<br>
Execute `su` again<br>
Execute `pm disable com.lenovo.ota`

# google apps are unable to connect to internet
ZUI install google framework already, however, it is disabled by default and version is behind<br>
Enable google framework from settings -> apps<br>
ZUI bases on Android 13, download the apk and install it, google services will work again<br>
`https://www.apkmirror.com/apk/google-inc/google-services-framework/google-services-framework-13-release/`
