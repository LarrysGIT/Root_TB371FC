# Root_TB371FC
这个教程教你如何root联想小新pad pro 12.7以及9008救砖 以及一些小问题

# 下载工具
## platform-tools
`https://developer.android.com/tools/releases/platform-tools`<br>
里面包含adb fastboot命令行工具 以及安卓设备驱动

## 高通9008救砖工具和驱动
`https://qfiltool.com`<br>
下载完成后安装QPST.2.7.496.1.exe和\Driver\Qualcomm USB Driver V1.0.exe

## root工具apk文件
`https://github.com/topjohnwu/Magisk/releases`

# 下载ZUI ROM
联想的ROM官网不提供下载 目前流出了3个版本的ROM我都在xda论坛上找到的<br>
它们也是目前某些平台用来卖钱的所谓救砖包<br>
ZUI 15.0.154开发版(不用下载只是放这做记录)<br>
`https://mirrors.lolinet.com/firmware/lenowow/Tab_P12_Pro_2023/TB371FC/`<br>
ZUI 15.0.405(建议下载)<br>
`https://www.mediafire.com/file/vdfae5lei59frbo/TB371FC-ZUI_15.0.405-QFIL.zip/file`<br>
ZUI 15.0.440(建议下载 这是首个支持了PC模式的版本 虽然并没有什么用)<br>
`https://www.mediafire.com/file/k7pjpl2841rb8gl/TB371FC-ZUI_15.0.440_FASTBOOT_QFIL_v2.zip/file`<br>
我是先刷了405 -> 再刷了440 (数据可以保留)<br>
你也可以选择直接440只是我没验证过

# 为什么要下载ROM刷机? 不能直接root现有的系统吗?
因为root工具Magisk需要ROM相应版本的boot.img文件 而联想并不提供ROM下载<br>
出厂版本的系统拿不到boot.img

# 解锁开发者模式
首先进平板的设置 -> 关于 -> 软件版本(zui 15.0.xxx) 一顿猛点 -> 系统提示处于开发者模式<br>
通用 -> 开发者选项 -> 打开usb调试 -> 连上电脑 -> 确认信任该计算机

# 解锁bootloader
*该步骤会导致失去保修且解锁后就无法再重新锁上*<br>
执行`adb reboot bootloader`进入fastboot模式 面板上会显示该平板的序列号(SN)<br>
拿着这个SN去联想官网申请解锁bootloader 联想会给你发一个邮件链接如下<br>
`http://cdn.zui.lenovomm.com/developer/tabletboot/SN号/sn.img`<br>
如果等不了联想给你发邮件 也可以通过修改这个repo里的`sn_AAAAAAAA.img`文件<br>
用16进制编辑器打开这个文件 找到AAAAAAAA的字符串 然后修改为自己平板的SN号保存即可<br>
运行`fastboot flash unlock sn.img`刷入下载的sn.img<br>
运行`fastboot reboot`重启 屏幕上会提示设备已经解锁

# 9008模式刷机OR救砖
打开电脑上的设备管理器<br>
断开平板USB线<br>
从系统里关机或者bootloader模式下选择关机 总之关机<br>
打开QFIL.exe工具进行如下配置<br>
- configuration -> Firehose configuration<br>
- - Device type 选择 "ufs"<br>
- - Validation mode 选择 "0 - no validation"<br>
- - Reset after download 打勾<br>
- - 点OK确定<br>
- Build type 选择"Flat Build"<br>
- Programmer 选择ROM 405目录下的`prog_firehose_ddr.elf`<br>
- Load XML 选择ROM 405目录下*所有列出来的xml文件*<br>

按住音量+键不要松 把平板连接上USB<br>
此时设备管理器里COM设备下会出现一个9008的设备 此时可以松开音量键<br>
回到QFIL.exe工具点击Download按钮 会开始刷机

# 安装root工具Magisk并root
重新执行`解锁开发者模式`步骤 连上USB<br>
把Magisk-v27.0.apk和对应ROM的boot.img传到平板上<br>
安装Magisk-v27.0.apk 安装完后打开应用<br>
选择install -> select and patch a file -> 选择boot.img<br>
从平板上把修改过的magisk_patched_xxxxxxx.img复制出来<br>
执行`adb reboot bootloader`进入fastboot模式<br>
执行`fastboot flash boot magisk_patched_xxxxxxx.img`刷入boot.img<br>
再进系统运行Magisk 就发现superuser按钮已经点亮<br>
root已经完成

# 从ROM 405升级到440
升级系统会丢失root 需要重新走一遍*安装root工具Magisk并root*步骤<br>
执行`adb reboot bootloader`后进入fastboot模式<br>
执行解压440的ROM后 里面名称为flash_all.bat的脚本即可

# 禁用系统更新
执行`adb shell`<br>
执行`su`此时会提示permission denied<br>
在Magisk中允许shell的root权限<br>
重新执行`su`<br>
执行`pm disable com.lenovo.ota`

# google服务一直提示连不上服务器
ZUI本身内置了google框架 但处于禁用状态 版本也太低<br>
启用并更新google framework即可 ZUI基于Android 13<br>
随便下载一个安装<br>
`https://www.apkmirror.com/apk/google-inc/google-services-framework/google-services-framework-13-release/`

# 其它ZUI版本的boot.img
XDA上有用户从4PDA论坛上转了几个ZUI系统版本的boot.img<br>
- boot_543orig.img<br>
- boot_575orig.img<br>
- boot_650orig.img<br>
https://xdaforums.com/t/tb371fc-xiaoxin-pad-pro-2023-12-7-sharing-and-support.4642987/page-23#post-89442944
