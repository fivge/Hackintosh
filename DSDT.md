## DSDT

<https://www.tonymacx86.com/threads/guide-patching-laptop-dsdt-ssdts.152573/>

<https://www.tonymacx86.com/threads/quick-guide-to-generate-a-ssdt-for-cpu-power-management.177456/>

### Maciasl

<https://bitbucket.org/RehabMan/os-x-maciasl-patchmatic/downloads>

### iasl

<https://bitbucket.org/RehabMan/acpica/downloads/>

```shell
sudo cp /Users/lux/Downloads/iasl  /usr/bin
```

### 提取

[[Guide] Patching LAPTOP DSDT/SSDTs](https://www.tonymacx86.com/threads/guide-patching-laptop-dsdt-ssdts.152573/)

`refs.txt`

```
External(MDBG, MethodObj, 1)
External(_GPE.MMTB, MethodObj, 0)
External(_SB.PCI0.LPCB.H_EC.ECWT, MethodObj, 2)
External(_SB.PCI0.LPCB.H_EC.ECRD, MethodObj, 1)
External(_SB.PCI0.LPCB.H_EC.ECMD, MethodObj, 1)
External(_SB.PCI0.PEG0.PEGP.SGPO, MethodObj, 2)
External(_SB.PCI0.GFX0.DD02._BCM, MethodObj, 1)
External(_SB.PCI0.SAT0.SDSM, MethodObj, 4)
External(_GPE.VHOV, MethodObj, 3)
External(_SB.PCI0.XHC.RHUB.TPLD, MethodObj, 2)
```

```shell
iasl -da -dl -fe refs.txt DSDT.aml SSDT*.aml
```



> DSDT.dsl

![](https://ws1.sinaimg.cn/large/006tNc79ly1fnrrzk5d7bj313r0gnagb.jpg)

![](https://ws3.sinaimg.cn/large/006tNc79ly1fnrryz46vsj31510gw7ah.jpg)

![](https://ws4.sinaimg.cn/large/006tNc79ly1fnrrzykhxjj30wj0fkjxh.jpg)



> *Just remove the line causing the error. This is actually a mistake by the person that wrote this code... The ACPI interpreter will simply ignore it and move on, so deleting it has the same effect as before this was made an error.*
>
> <https://www.tonymacx86.com/threads/how-to-fix-this-error-invalid-type-target-is-package-source-must-be-a-package-also.195071/>

---

> *Clover: Files should be placed on the Clover bootloader partition (usually the EFI partition), in EFI/Clover/ACPI/patched. DSDT.aml, if present, will automatically replace the OEM DSDT. This guide (and other guides linked from this guide) assumes you are using config.plist/ACPI/AutoMerge=true, config.plist/ACPI/SSDT/DropOem=false. With AutoMerge=true, patched SSDTs can be placed in ACPI/patched with their original name (from ACPI/patched) and they will be inserted such that the original order of SSDTs is not disturbed. It is not necessary (or advised) to use SortedOrder with AutoMerge=true. Other configurations are covered below in "Recommended configurations".*