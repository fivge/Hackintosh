### 0x01 "Window Server Service only ran for 0 seconds" with dual-GPU

<https://www.tonymacx86.com/threads/fix-window-server-service-only-ran-for-0-seconds-with-dual-gpu.233092/>



- use Clover F4 to extract native ACPI files to ACPI/origin
- disassemble the files
- search for a method named _OFF
- examine the resulting files to determine the ACPI path

> ##### 反编译

```bash
iasl -dl DSDT.aml SSDT*.aml

grep -l Method.*_OFF *.dsl


```

```
// save as SSDT-DiscreteSpoof.aml
DefinitionBlock ("", "SSDT", 2, "hack", "spoof", 0)
{
    Method(_SB.PCI0.PEG0.PEGP._DSM, 4)
    {
        If (!Arg2) { Return (Buffer() { 0x03 } ) }
        Return (Package()
        {
            "name", Buffer() { "#display" },
            "IOName", "#display",
            "class-code", Buffer() { 0xFF, 0xFF, 0xFF, 0xFF },
        })
    }
}
```

> ##### 编译

```bash
iasl SSDT-DiscreteSpoof.dsl

```



>  *This method uses a _DSM method in an SSDT to inject the properties. If your native ACPI has an existing _DSM method at that path, you will need to rename it, because otherwise the native _DSM conflicts with the _DSM the SSDT is adding. Typically, this is done by entering a _DSM->XDSM patch in your config.plist/ACPI/DSDT/Patches. This patch is provided in all my guide plists, but is disabled. *

### 0x02

> Also, once you enable the patch, you still may have panic... It is because Clover cannot patch a kext that loads outside of kernel cache, and these graphics kexts may not be in cache. To work around this problem, use an invalid ig-platform-id (0x12345678). DO NOT use a bogus FakeID, as that will defeat the purpose. Once you are able to boot with the invalid ig-platform-id, rebuild cache, then boot normally with your intended ig-platform-id. 

> **Q. Do I need to set DVMT-prealloc to 64MB?**  
>
> Setting DVMT-prealloc is the preferred way to overcome the assumptions made by the graphics kexts. But you can also work around it by patching the framebuffer to match your BIOS setting (32mb on many Broadwell/Skylake/KabyLake).  [https://www.tonymacx86.com/threads/...lensize-patch-with-32mb-dvmt-prealloc.221506/](https://www.tonymacx86.com/threads/guide-alternative-to-the-minstolensize-patch-with-32mb-dvmt-prealloc.221506/)   
>
> **Q. Is Skylake supported?**  
>
> Yes,... but be aware of DVMT-prealloc requirements for integrated graphics.  Read questions below.
>
> <https://www.tonymacx86.com/threads/faq-read-first-laptop-frequent-questions.164990/>

### 0x0

```
安装10.13时卡在Service only ran for 0 seconds. Pushing respawn out by 10 second
此种现象常见于笔记本机型，由于10.13中的DSDT屏蔽独显方式失效，现使用 hotpatch 方式进行独显屏蔽。

使用方法：
将 SSDT-Disable-DGPU.aml 复制到 /EFI/CLOVER/ACPI/patched 目录下即可
```



### 0x02 亮度调节

[Laptop backlight control using AppleBacklightInjector.kext](https://www.tonymacx86.com/threads/guide-laptop-backlight-control-using-applebacklightinjector-kext.218222/)



### 0x0

```bash
ioreg -lw0 | grep -i "IODisplayEDID" | sed -e 's/.*<//' -e 's/>//'
00ffffffffffff000daec3140000000026190104951f1178027e45935553942823505400000001010101010101010101010101010101da1d56e250002030442d470035ad10000018000000fe004e3134304247412d4541330a20000000fe00434d4e0a202020202020202020000000fe004e3134304247412d4541330a200053
➜  Hackintosh git:(master) ✗ ioreg -l | grep "DisplayVendorID" 
    | |   | | |       "DisplayVendorID" = 3502
➜  Hackintosh git:(master) ✗ ioreg -l | grep "DisplayProductID" 
    | |   | | |       "DisplayProductID" = 5315
```



