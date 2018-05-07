## config.plist

###### ig-platform-id

> In any case you may need to change the ig-platform-id that is used at /Graphics/ig-platform-id. But these configurations work most of the time.



> Common ig-platform-ids:
> 0x01660003: HD4000 1366x768
> 0x01660004: HD4000 1600x900, 1920x1080
> 0x01660008, 0x01660009: HD4000 1600x900, 1920x1080
>
> 0xa260006: HD4400/HD4600/HD5000
> Other Haswell ig-platform-id values: 0xa260005, 0xa260000, 0xa160000, 0xa2e0008, 0xa2e000a

> **Note: HD4200, HD4400, and HD4600 on 10.10+ needs special patches/injections, thus the separateconfig_HD4600_4400_4200.plist.**

### SSDTs

If you're getting a panic in AppleIntelCPUPowerManagement and/or SMC_ACPI_PlatformPlugin it may be related to your OEM CPU power management related SSDTs.

Some systems may need to drop some of the OEM SSDTs. This happens most frequently with Sandy Bridge systems (but not all). There are two configurations for DropTables in the provided config.plist files. The default is minimal. The alternate is named #DropTables and is a bit more aggressive. Each configuration resides in config.plist/ACPI. You can use the alternate by renaming DropTables->##DropTables and renaming #DropTables->DropTables (in that order). Depending on how the OEM labels the tables, this may or may not work. If you still have issues, set config.plist/ACPI/SSDT/DropOem=true. You will need to set config.plist/ACPI/SSDT/Generate=true (or the individual CStates/PStates=true) to use DropOem=true or the alternate DropTables.



---



从10.11 DB5/PB3版开始，rootless=0以及kext-dev-mode=1启动参数已经被废除，请不要再使用



---

经测试最新4296开始可解决a卡如r9 290x RX480 RX470 RX580 RX570等单卡启动问题以及a卡睡眠唤醒黑屏，只需加入一个参数即可丢掉dsdt和hotpatch以及whatevergreen，CloverConfigurator勾选
**RadeonDeInit即可**

---

如果需要HDMI/DP音频输出，则可在clover configurator的ACPI里勾选FIXDisplay +Add HDMI，无需其他ssdt和kext即可生效

---

Q1. 10.13 安装发生 An error occurred while verifying firmware 更新固件时发生错误 错误?
A1. 1.自行使用 Clover Configurator 设置各版本最新的 SMbiosversion, FirmwareFeatures, FirmwareFeaturesMask。
　　2.无法使用 Win GPT EFI分区 更新固件, 请备份所有磁盘文件, 并使用 Mac磁盘工具, 重新划分 Mac GPT分区表, 
　　　自动新建 Mac GPT EFI分区。
　　3.或用 [制作 10.13 安装盘 (适用于 MBR 安装)](http://bbs.pcbeta.com/viewthread-1746351-1-1.html) 的 Disk1mbrInstaller 进行第二阶段安装, 略过 firmware 错误。
　　 ![img](http://bbs.pcbeta.com/static/image/filetype/zip.gif) [firmware-all-10131.zip](http://bbs.pcbeta.com/forum.php?mod=attachment&aid=Mzk4OTAwNnxmODRmODIyZXwxNTIyNTg2ODE2fDQ2MzQxNDh8MTY5NTkzMA%3D%3D) *(221.88 KB, 下载次数: 266) *
　　 ![img](http://bbs.pcbeta.com/static/image/filetype/zip.gif) [Clover Configurator_v4.54.0.0_10.10+.zip](http://bbs.pcbeta.com/forum.php?mod=attachment&aid=MzkwNzAyOXxiZWYxZTQxZnwxNTIyNTg2ODE2fDQ2MzQxNDh8MTY5NTkzMA%3D%3D) *(2.89 MB, 下载次数: 3211) *
Q2. 发生 "System/Installation/Packages/OSInstall.mpkg" appears to be missing or damaged 似乎已缺失或已损坏 错误?
A2. 1.[删掉 /EFI/CLOVER/drivers64UEFI/EmuVariableUefi-64.efi 及 /NVRAM.plist](http://bbs.pcbeta.com/viewthread-1756719-1-1.html), 安装完后, 可恢复使用 (GPT为EFI分区)
　　2.移除其他 HDD 及 SSD 磁盘, 或在 BIOS 设置 Disable, 只保留 要安装的磁盘。
　　3.如果还是持续错误, 窗口->日志->显示所有日志, 请提取 安装日志 install log, 发帖询问错误原因。
Q3. 安装时，如何禁止 SSD 自动转换为 APFS 格式?
A3. 重新安装，格盘为 Mac OS 扩展(日志式) 分区，第二阶段安装前，修改 true 为 false，可取消转换为 APFS 。
　　/macOS Install Data/minstallconfig.xml
　　    <key>ConvertToAPFS</key>
　　    <false/>



---

自 r4296 起只需删除WhateverGreen.kext并在config.plist中添加<key>RadeonDeInit</key>参数即可解决A卡启动问题以及睡眠问题

自 r4301 起支持Fusion Drive(10.12+)

自 r4378 起pkg安装包添加OsxAptioFix3Drv.efi

自 r4410 起EFI添加OsxAptioFix3Drv.efi（会与OsxAptioFixDrv.efi冲突，请选择其中一个使用）



---

