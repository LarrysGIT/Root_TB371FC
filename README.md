# Root_TB371FC
Lenovo XiaoXin Pad Pro 2023 12.7 root and rescue
2024年我不知道你们root干嘛 我个人是用来跑linux deploy的container
ipad坏了之后刚好朋友在国内 我就让带了这个安卓平板过来
这个教程教你如何root联想小新pad pro 12.7以及9008救砖 以及一些小问题
所有URL默认都是https除非特别写明

# 下载工具
## platform-tools
`developer.android.com/tools/releases/platform-tools`
里面包含adb fastboot命令行工具 以及安卓设备驱动

## 高通9008救砖工具和驱动
`qfiltool.com`
`androiddatahost.com/nbyn6`

## root工具apk文件
`github.com/topjohnwu/Magisk/releases`

# 下载ZUI ROM
联想的ROM官网不提供下载 目前流出了3个版本的ROM我都在xda论坛上找到的
它们也是目前某些平台用来卖钱的所谓救砖包
ZUI 15.0.154开发版(不用下载只是放这做记录)
`mirrors.lolinet.com/firmware/lenowow/Tab_P12_Pro_2023/TB371FC/`
ZUI 15.0.405(建议下载)
`www.mediafire.com/file/vdfae5lei59frbo/TB371FC-ZUI_15.0.405-QFIL.zip/file`
ZUI 15.0.440(建议下载 这是首个支持了PC模式的版本 虽然并没有什么用)
`www.mediafire.com/file/k7pjpl2841rb8gl/TB371FC-ZUI_15.0.440_FASTBOOT_QFIL_v2.zip/file`
我是先刷了405 -> 再刷了440 (数据可以保留)
你也可以选择直接440只是我没验证过

# 为什么要下载ROM刷机? 不能直接root现有的系统吗?
因为root工具Magisk需要ROM相应版本的boot.img文件 而联想并不提供ROM下载
出厂版本的系统拿不到boot.img

# 解锁开发者模式
首先进平板的设置 -> 关于 -> 软件版本(zui 15.0.xxx) 一顿猛点 -> 系统提示处于开发者模式
通用 -> 开发者选项 -> 打开usb调试 -> 连上电脑 -> 确认信任该计算机

# 解锁bootloader
*该步骤会导致失去保修且解锁后就无法再重新锁上*
执行`adb reboot bootloader`进入fastboot模式 面板上会显示该平板的序列号(SN)
然后修改如下的链接 sn号改为你自己平板的 然后粘贴到浏览器里下载sn.img (手动加上前缀http://)
`cdn.zui.lenovomm.com/developer/tabletboot/SN号/sn.img`
运行`fastboot flash unlock sn.img`刷入下载的sn.img
运行`fastboot reboot`重启 屏幕上会提示设备已经解锁

# 9008模式刷机OR救砖
打开电脑上的设备管理器
断开平板USB线
从系统里关机或者bootloader模式下选择关机 总之关机
打开QFIL.exe工具进行如下配置
  configuration -> Firehose configuration
    Device type 选择 "ufs"
    Validation mode 选择 "0 - no validation"
    Reset after download 打勾
    点OK确定
  Build type 选择"Flat Build"
  Programmer 选择ROM 405目录下的`prog_firehose_ddr.elf`
  Load XML 选择ROM 405目录下*所有列出来的xml文件*
按住音量+键不要松 把平板连接上USB
此时设备管理器里COM设备下会出现一个9008的设备 此时可以松开音量键
回到QFIL.exe工具点击Download按钮 会开始刷机

# 安装root工具Magisk并root
重新执行`解锁开发者模式`步骤 连上USB
把Magisk-v27.0.apk和对应ROM的boot.img传到平板上
安装Magisk-v27.0.apk 安装完后打开应用
选择install -> select and patch a file -> 选择boot.img
从平板上把修改过的magisk_patched_xxxxxxx.img复制出来
执行`adb reboot bootloader`进入fastboot模式
执行`fastboot flash boot magisk_patched_xxxxxxx.img`刷入boot.img
再进系统运行Magisk 就发现superuser按钮已经点亮
root已经完成

# 从ROM 405升级到440
升级系统会丢失root 需要重新走一遍*安装root工具Magisk并root*步骤
执行`adb reboot bootloader`后进入fastboot模式
执行解压440的ROM后 里面名称为flash_all.bat的脚本即可

# 禁用系统更新
执行`adb shell`
执行`su`此时会提示permission denied
在Magisk中允许shell的root权限
重新执行`su`
执行`pm disable com.lenovo.ota`

# google服务一直提示连不上服务器
ZUI本身内置了google框架 但处于禁用状态 版本也太低
启用并更新google framework即可 ZUI基于Android 13
随便下载一个安装
`www.apkmirror.com/apk/google-inc/google-services-framework/google-services-framework-13-release/`
