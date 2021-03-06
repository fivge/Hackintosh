## Kexts

###### 0x01 内核

<https://bitbucket.org/RehabMan/os-x-fakesmc-kozlek/downloads>

> Note: The FakeSMC package includes FakeSMC "plugins" (FakeSMC_ACPISensors.kext, FakeSMC_CPUSensors.kext, FakeSMC_LPCSensors.kext, FakeSMC_GPUSensors.kext). You do not need these kexts for installation, although you may wish to try them for getting sensor data to HWMonitor.app after you install. Note: FakeSMC_CPUSensors.kext may have issues on Kaby Lake hardware.

###### 0x02 万能键鼠

<https://bitbucket.org/RehabMan/os-x-voodoo-ps2-controller/downloads>

[如何使用自定义的按键映射功能](https://github.com/RehabMan/OS-X-Voodoo-PS2-Controller/wiki/如何使用自定义的按键映射功能)

[how to install](https://github.com/RehabMan/OS-X-Voodoo-PS2-Controller/wiki/How-to-Install)

- install the VoodooPS2Daemon

```bash
sudo cp org.rehabman.voodoo.driver.Daemon.plist /Library/LaunchDaemons

sudo cp VoodooPS2Daemon /usr/bin
```

###### 0x03 USBInjectAll.kext

> ###### 详见kexts

###### 0x04 Lilu && IntelGraphicsFixup

For 10.12.5 and HD515/HD520/HD530/HD615/HD620/HD630, you will likely need Lilu.kext and IntelGraphicsFixup.kext:
Lilu.kext: <https://bitbucket.org/RehabMan/lilu/downloads/>
IntelGraphicsFixup.kext: <https://bitbucket.org/RehabMan/intelgraphicsfixup/downloads/>

###### 0x05 有线网卡

[RTL8168/8111](https://bitbucket.org/RehabMan/os-x-realtek-network/downloads)

Intel   <https://github.com/RehabMan/OS-X-Intel-Network>

<https://bitbucket.org/RehabMan/os-x-intel-network/downloads/>



----

Haswell HD4200/HD4400/HD4600: Must inject device-id=0x0412.
config.plist/Devices/FakeID/IntelGFX=0x04128086

###### 0x06 FakePCIID.kext

<https://bitbucket.org/RehabMan/os-x-fake-pci-id/downloads/>

<https://github.com/RehabMan/OS-X-Fake-PCI-ID>

> *In addition, the Haswell graphics accelerator kext (AppleIntelHD5000Graphics.kext) checks the PCI device-id against a set of known values. Because the HD4200/HD4400/HD4600(mobile) values are not accommodated by this check, you must install FakePCIID kexts for PCI device-id spoofing.*

> *Install both FakePCIID.kext and FakePCIID_Intel_HD_Graphics.kext (originally FakePCIID_HD4600_HD4400.kext) in /System/Library/Extensions, EFI/Clover/kexts (typically in EFI/Clover/kexts/Other), or /Library/Extensions (10.11+).*

> HD4200/HD4600/HD4400 mobile: By using the config.plist for HD4600, you'll have CI, but not QE. HD4400 has QE/CI but there will be other problems in the system (Preview/Safari/etc). You will need to install the two kexts (FakePCIID.kext, FakePCIID_HD4600_HD4400.kext) 

---



######  0x07 亮度调节驱动

`AppleBacklightInjector.kext`

> ###### 详见kexts

###### 0x08 电池(待补充)

<https://bitbucket.org/RehabMan/os-x-acpi-battery-driver/downloads>

**配合DSDT**

<http://www.tonymacx86.com/yosemite-laptop-support/116102-guide-how-patch-dsdt-working-battery-status.html>

###### 0x09声卡驱动

<https://github.com/vit9696/AppleALC>

<https://blog.daliansky.net/Use-AppleALC-sound-card-to-drive-the-correct-posture-of-AppleHDA.html>

`Xcode`编译,联合`Lilu`debug版.

修改clover中的声卡`layout id`

![](https://ws2.sinaimg.cn/large/006tKfTcly1fr1qomeq38j30nn0d6wgy.jpg)

**查看效果**

```bash
kextstat | grep AppleALC

ioreg -l | grep layout-id
```

###### 0x10 万能声卡驱动

<http://sourceforge.net/projects/voodoohda/files/?source=navbar>

`ALC269VC`

######  0x011 AR956X AR946X AR9485(无线网卡WiFi模块)

> ###### 方法一

<https://github.com/athlonreg/Enable-AR956X-AR946X-AR9485-in-your-hacintosh>

<https://www.insanelymac.com/forum/forums/topic/312045-atheros-wireless-driver-os-x-101112-for-unsupported-cards/?do=findComment&comment=2509900>

> ###### 方法二

 <https://github.com/chunnann/ATH9KFixup>

<https://bitbucket.org/RehabMan/ath9kfixup/overview>

