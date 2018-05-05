### 0x01 新手上路

**most important**

<https://www.tonymacx86.com/threads/guide-booting-the-os-x-installer-on-laptops-with-clover.148093/>

### 0x02 Clover

###### Clover官方下载(不推荐)

<https://sourceforge.net/projects/cloverefiboot/>

###### Clover_RahabMan编译的

<https://bitbucket.org/RehabMan/clover/downloads/>

### 0x03 Clover配置文件

通用配置文件

<https://github.com/RehabMan/OS-X-Clover-Laptop-Config>

### 0x04 kexts

~~(详见kexts)~~

### 0x05 drivers

<http://bbs.pcbeta.com/forum.php?mod=viewthread&tid=1775839&highlight=drivers64UEFI>

<https://clover-wiki.zetam.org/zh-CN/What-is-what#CloverEFI%E7%9A%84%E5%90%AF%E5%8A%A8>

<https://www.insanelymac.com/forum/forums/topic/317290-filevault-2/>

> >  #### Notes on HDD install:

##### EmuVariableUefi-64.efi

you might want "EmuVariableUefi-64.efi", but it would depend on whether native NVRAM works for you (most Skylake hardware has non-functional native NVRAM with OS X/macOS)

##### AptioMemoryFix.efi

#####OsxAptioFix*.efi

内存修复

Note: AptioMemoryFix.efi instead of OsxAptioFix*.efi may allow native NVRAM to work on platforms where it typically does not. AptioMemoryFix.efi is rather new, no official build (as I write this), but source is available (<https://github.com/vit9696/AptioFixPkg>). In the attachments, you will find a build I created by running the macbuild.tool script.

##### EmuVariableUefi-64

macOS使用NVRAM存储一些系统变量，大部分的UEFI主板在配合合适的Aptio驱动后支持原生硬件NVRAM，但是少部分主板不支持NVRAM或者NVRAM的支持有问题，此时建议加入该驱动，该驱动通过开机时加载位于EFI分区内的nvram.plist内容到nvram中，以模拟NVRAM支持
需要注意的是，是用此驱动，需要勾选“安装RC scripts到目标磁区”选项才有效

##### DataHubDxe-64.efi

DataHub协议是MacOSX的强制支持的。通常它是已经存在的，但有时它可能会丢失，在这种情况下，你应该看到屏幕上的警告信息。该文件的存在始终是安全的

> > #### All



##### APFS.efi

> You can find apfs.efi at /usr/standalone/i386/apfs.efi inside of "/Applications/Install macOS High Sierra.app/Contents/SharedSupport/BaseSystem.dmg".

##### HFSPIus.efi

<https://github.com/JrCs/CloverGrowerPro/raw/master/Files/HFSPlus/X64/HFSPlus.efi>

##### VBoxHfs-64.efi(没用)

![](https://ws2.sinaimg.cn/large/006tKfTcly1fnpuhjxxvoj30ld0c1q3w.jpg)

> Note: You may have VboxHfs-64.efi there too. But it doesn't matter. It will be disabled by the config.plist. If you want to use VboxHfs-64.efi you will need to remove the disabler entry for it in config.plist/DisableDrivers.

##### NTFS.efi?

### 0x06 patched

SSDT-DiscreteSpoof.aml

SSDT-PNLF.aml

