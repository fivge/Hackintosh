### 0x01 "Window Server Service only ran for 0 seconds" with dual-GPU

<https://www.tonymacx86.com/threads/fix-window-server-service-only-ran-for-0-seconds-with-dual-gpu.233092/>



- use Clover F4 to extract native ACPI files to ACPI/origin
- disassemble the files
- search for a method named _OFF
- examine the resulting files to determine the ACPI path



```she
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

>  *This method uses a _DSM method in an SSDT to inject the properties. If your native ACPI has an existing _DSM method at that path, you will need to rename it, because otherwise the native _DSM conflicts with the _DSM the SSDT is adding. Typically, this is done by entering a _DSM->XDSM patch in your config.plist/ACPI/DSDT/Patches. This patch is provided in all my guide plists, but is disabled. *

### 0x02 亮度调节

### [Laptop backlight control using AppleBacklightInjector.kext](https://www.tonymacx86.com/threads/guide-laptop-backlight-control-using-applebacklightinjector-kext.218222/)



