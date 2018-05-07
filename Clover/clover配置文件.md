# Clover配置文件

---

+ 自行加入以下参数到 config.plist，关闭 SIP 部分功能。

```
	<key>RtVariables</key>
    <dict>
        <key>BooterConfig</key>
        <string>0x28</string>
        <key>CsrActiveConfig</key>
        <string>0x67</string>
    </dict>
```

---

### ACPI

+ HaltEnabler 选项

  参数名称:HaltEnabler  参数设置:<true/> 开启 || <false/> 关闭  参数说明:建议开启 这个参数等同于OpenHaltRestart驱动的作用,就是在OSX系统启动时清除SLP_SMI_EN,以解决重启/关机时遇到无法断电问 题。  Intel7系主板此项与SuspendOverride、SlpSmiAtWake同时使用,解决睡眠唤醒问题。 Intel8系主板此项与SlpSmiAtWake同时使用,解决睡眠唤醒问题。
  
+ DSDT
  + Name
  参数设置:< DSDT.aml>使  定义DSDT || < 空> 不使  定义DSDT 参数说明:加载和注 名称为DSDT的 进制 件(这 指  提取的dsdt 件)  
  + ~~FixMask~~
  + 参数名称:Fixes
      参数设置:N/A 参数说明:此参数是 个全套的DSDT修复补丁,可以单独激活。包括“oldWay"和“NewWay”选项
    0xffffffff
  + Patches 子项 对DSDT打二进制补丁
  + 参数名称:DropOEM_DSM参数设置:<true/> 开启 || <false/> 关闭参数说明:一些OEM DSDT已经包含了某些设备的Method(_DSM...) 函数,但它的另一个结构,另一个逻辑和另一个结果是 我们需要的。我们不能修改这个函数,我们不可以使用相同的名称来创建这个函数本身,所以使用DropOEM_DSM会创建替 换掉这些OEM _DSM。 如果使用自定义的DSDT.aml此项将默认关闭,如果使用BIOS.aml将开启修复。(官方wiki未更新)新版修复功能已改变

  + 参数名称:SlpSmiAtWake参数设置:<true/> 加入 || <false/> 不加入 参数说明:在每次唤醒时加入SLP_SMI_EN=0参数。它可以帮助解决UEFI引导模式下的睡眠和关机的问题

  + 参数名称:SuspendOverride休眠覆写参数设置:<true/> 加入 || <false/> 不加入参数说明:影响 FixShutdown_0004的DSDT补丁,并扩展到从状态5到3,4和5状态(睡眠和休眠)的修复

  + ReuseFFFF
      参数设置:<true/> 注入 || <false/> 不注入
      参数说明:一些OEM DSDT包含一些名称为(_adr,0xFFFF)的设备。这是一个很大的问题,我可以把它转化为ADR = 0并 注入属性,但这是危险的补丁,可能会导致  IOPCIFamily.kext不能正常工作。所以这个值提出了将此设备改为(ADR,0)并 重新注入设备。(例如FakeID)
    
+ SSDT 

  参数设置:N/A 参数说明:设置SSDT组参数
  
  + DropOem
      参数设置:<true/>开启 || <false/> 关闭
    	参数说明:建议开启
		当为你的处理器生成一个包含P -和C-状态的SSDT时将获取所有内部SSDT表单以避免产生冲突。Clover可以自动完成这些工 作,或者你可以指定一个外部文件,将从EFI / OEM / [model] / ACPI /patched目录中加载它。 PS:model即为“SystemProductName”,你的主机板名,可以从dsdt.aml查得和clover侦测获得。 DropOem与CStates,PStates 组合使用
  
  + UseSystemIO
  	参数设置:<true/> 开启 || <false/> 关闭
		参数说明:尚无详解,建议开启
		使用UseSystemIO 开关的作用是对SSDT部分里生成的_cst表单“Register (FFixedHW, ”和“Register (SystemIO,”之间进行选择
	这个参数其实是使用系统IO来变频,FFixedHW由硬件来控制,一般来说效率比较高的是FFixedHW
  
  + Generate
  	参数设置:N/A 必选参数 参数说明:生成一个含有P -和C-状态得SSDT文件
	+ CStates
	  参数设置:<true/>开启 || <false/> 关闭
	  	  参数说明:必选参数
	  	  为每个处理器核心部分加入_CST 函数,自动生成SSDT表单。
	  	  参数EnableC2, EnableC4, EnableC6, EnableISS, C3Latency. 会影响_CST的生成。没有必要评价这些参数都会起作用,需要 你自己通过实验确定。
	  	  此外,Clover已经获得了处理器的类型和核心数信息,不使用此参数将导致以下错误信息:
	  `ACPI_SMC_PlatformPlugin::pushCPU_CSTData - _CST evaluation failed.`
	+ PStates
		  参数设置:<true/>开启 || <false/> 关闭
	  	  参数说明:必选参数
	  	  为处理器加入 _PPC, _PPC and _PSS 函数,自动生成SSDT表单。函数作用如下:
	  	  _PCT - 性能控制。控制SpeedStep功能
	  	  _PPC - 当前的性能及SpeedStep能力。
	  	  这个函数通过返回一个数值来限制CPU频率,进一步了解见 PLimitDict 项设置 Haswell处理器需要配合fix_acst_4000000来满足SpeedStep(变频节能要求)
	  详〔 Rev 2422 ,2420 〕说明 _PSS -性能的支持状态。一个可能包含CPU- p状态的数组(array)。
	  	  	  当生成这个数组(array)时,PLimitDict, UnderVoltStep 和 Turbo 的设置要被考虑进来
  +  EnableC2 参数设置:<true/>开启 || <false/> 关闭 参数说明:建议开启这个键值可以开启 CPU的C2状态,默认为禁用
  + EnableC4
	参数设置:<true/>开启 || <false/> 关闭 参数说明:建议开启这个键值可以开启 CPU的C4状态,默认为禁用
  + EnableC6
	参数设置:<true/>开启 || <false/> 关闭 参数说明:SandyBridge Ivy Bridge 处理器建议开启 这个键值可以开启 CPU的C6状态,默认为禁用
	  + EnableC7
	参数说明:HasWell 处理器建议开启 这个键值可以开启 CPU的C6状态,默认为禁用
  + UnderVoltStep参数设置:<value>数值参数说明:建议移动平台开启,慎用 此参数可降低CPU电压并间接影响到温度高低。可能的数值是0,1,2等。Clover将只允许正常的值,这意味着它将安全地增 加这个数值直到CPU停止工作。
  + MinMultiplier参数设置:<value>数值参数说明:CPU 最低倍频设置。通常情况这个值被赋予16, 产生一个1600 MHz 的频率,但当你要是开启SpeedStep(节能变 频功能)时,你要用小一点的数值如8或7Haswell处理器 设置 为8
  + MaxMultiplier参数设置:<value>数值 参数说明:一个类似于“minimal”的倍频器,但不是必需的。**不建议设置**
+ DropTables
  参数设置:此项需慎重,设置不当导致无法进入系统
  
  <初级设定>此项不设置,直接开启 SSDT / DropOem ,或配合外部获取的SSDT文件
  
  <高级设定>更新中。。。
  
### Boot
+ Arguments
  + Boot Flags
 	 -v 啰嗦模式启动:不能正常进入系统时采用,故障调试,显示所有调试日志(含错误信息)
  	-s 启动 OS X 进入 单用户模式:不能正常进入系统时采用,脱离图形界面,以命令行方式进入,不适合新手适用。
 	-x 启动 OS X 进入 SafeBoot(安全模式):不能正常进入系统时采用。
  +  npci
  两个参数不能同时启用,貌似0x2000针对旧型号显卡,自行测试效果。 npci=0x2000:解决独立显卡PCIconfiguration begin卡住不动错误。 npci=0x3000:解决独立显卡PCIconfiguration begin卡住不动错误。
  + nvda_drv=1:此参数可以让系统安装Nvidia官方WebDriver显卡驱动,作用等同于变色龙下”nvda_drv=1“参数。 Clover下此参数生效条件:需将SMBIOS设置为Mac Pro机型
  + nv_disable=1:此参数可以禁止nVidia独显驱动加载
  + Kernel=mach_kernel
  从系统内核启动,如果不加入此参数"Kernel=...",将默认加载系统缓存(kernelcache)启动,作用等同于启 动菜单的”without kernelcache“选项。
+ Legacy
  + LegacyBiosDefaultEntry参数设置:< 0/1/2/3...>硬盘编号,0 表示第1块硬盘参数说明:对于 UEFI 开机方式,你可以指定启动硬盘(不仅是第1个)。选择此项后,LegacyBiosDefaultEntry子项激 活。上例中,选2 表示从第3块硬盘启动
+ Timeout参数设置:<5/4/3/2/1/0/-1>计数单位:秒参数说明:Clover引导器在加载一个操作系统前将暂停 5 秒,如果在这段时间里按任一键,则倒计时将停止。选项定义如 下:5 - 暂停时间,单位:秒,可选5,4,3,2,1建议设置0 - 不使用GUI图形界面,直接加载操作系统 不适合新手-1 - 使用GUI图形界面, 不自动加载操作系统 建议设置 Clover必须找到一个默认选项,才能自动加载一个操作系统,具体设置见下面的“DefaultVolume”选项
+ DefaultVolume参数设置:一般设置卷名较便捷参数说明: 这里可以用卷名,分区GUID(完整的GPT分区路径),或唯一设备路径,用来告诉Clover哪项是默认加载引导 卷。同样功能见DefaultLoader 选项。下面是一些可以使用的数值范例:
  + 卷名 `Macintosh`
  +    GPT     `HD(1,GPT,57272A5A-7EFE-4404-9CDA-C33761D0DB3C,0x800,0xFF000)`
  + 唯一设备路径(UUID) `57272A5A-7EFE-4404-9CDA-C33761D0DB3C`
  + 上一次启动的启动项 `LastBootedVolume`
+ DefaultLoader参数设置:多分区多系统建议设置此项参数说明:除了上面DefaultVolume 选项,引导文件的路径可以被指定到DefaultLoader项里。这里提供了多分区多系统引 导时更精确的默认条目选择。该值可以是完整的路径或唯一设备路径(UUID)及文件名
  ```
   <key>DefaultLoader</key>  <string>BOOTX64.efi</string>
  ``` 
+ Log参数设置:<true/>开启 || <false/> 关闭 参数说明:在系统每次启动时保存日志文件。建议开启 如果你开机无法正常进入Clover,你可以使用这个设置生成一个调试结果输出到/EFI/CLOVER/misc/debug.log文件,这个日 志文件有助你分析出是哪里出了问题。警告!当它正在将日志文件写入磁盘时,打开日志将大大增加加载时间。因此在重启 电脑前,要有足够的耐心来等待。
+ Fast参数设置:<true/>开启 || <false/> 关闭 参数说明:此参数类似于设置Timeout=0,但有以下区别: nvram.plist文件只在设置后的第一次启动时被检测存储,后续开机则跳过检测,以加快开机速度。此参数生效后的效果: 1.不侦测最佳屏幕显示模式2.不加载图形界面主题3.不扫描内存SPD4.不能选择进入GUI图形界面**启动此参数对其他功能应该没有大的影响**  
+ Secure

### CPU

### Device

+ LpcTune
**建议关闭**
+ Inject

+ NoDefaultProperties
+ UseIntelHDMI
+ FakeID
FakeID参数设置:< 6位DeviceID+4位VendorID> 参数说明:该参数作用就是“仿冒驱动”。其原理是:通过对不支持设备注入苹果原生支持列表里相近的设备id,达到使用苹 果原生驱动使设备正常工作之目的。如下面的例子:AMDRadeonHD7850没有被支持,DeviceID = 0x6819。替换为0x6818(新版已支持)戴尔无线网卡1595,DeviceID = 0x4315不支持。替换为0x4312。网卡Marvell Yukon 8056,DeviceID = 0x4353。替换为0x4363。还有其他已知的不支持设备的替换。FakeID功能生效的条件: 只有当injectATI(NVIDIA,Intel)被设置或在FixDSDT面板里设置了该设备时,这种替换才生效。
+ Audio / Inject 子项
  No/Detect/layout id> 默认关闭 参数说明:这个参数作用不是使声卡工作,但它能帮助你,注入声音芯片属性。其中:“layout id”是指填入你能正常工作的 声卡的“LayoutID”。 该参数功能生效条件:当DSDT文件里定义了Device(HDEF)项,如果你重新命名它(指HDEF),你可以注入其他不同 的属性。使用VodooHDA驱动时,此参数无效。可用的注入参数如下:No – 关闭注入功能,对于使用DSDT注入和使用Voodoo驱动的可以设置此项。Detect –自动读取声音芯片设备ID,并将读取HEX(16进制)数值转换为Decimal(10进制)数值并作为Layout ID注入,如 果读取设备的HEX(16进制)的不能转换到10进制数值,Clover将注入Decimal(10进制)的”12”数字。其他所有的数值都可 以用“Detect“参数获取。此设置不会改变“系统信息”里”声音“的属性描述。883 – 填入Decimal(10进制)数值,如本例是realtek ALC883 声卡。0x373 – 填入HEX(16进制)数值,如本例是realtek ALC883 声卡。 如果上面填入的数值不正确,你需要自行找到正确的数值并替换AppleHDA驱动里的相应layout文件,来使声卡正常工作。
+ USB
  + Inject
     默认注入 参数说明:注入USB设备属性,如果你需要可以随时关闭,另外如果打了0X1000的DSDT修复补丁, 此功能也将被关闭。  + AddClockID
    默认关闭 参数说明:应根据你的使用需求设置,此参数作用是为每个唯一标识(UUID)的设备注入"AAPL,clock-id",以改变睡眠唤醒 方式。    <true/> -适合深度睡眠“Deep sleep”,不能用键盘或鼠标来唤醒。	<false/> -计算机能正常睡眠,可以用键盘或鼠标(或其他一些外部设备如网卡,USB设备等)唤醒。 注意!此参数生效条件:需要开启USB / Inject 子项,此参数默认是关闭的  + HighCurrent
    默认关闭 参数说明:为满足Ipad充电需求,提高USB端口电量输入能力


### DisableDrivers

### Graphics

+ Inject
  显卡注入功能
   + Intel核显注参数设置:<true/> 注  || <false/> 不注 参数说明:此参数通常会与 ig-platform-id 注  组合在 起 ,具体请看置顶集合贴V3 各核显的驱动教 程。
    0600260a
  + NVidia独显     参数设置:<true/> 注  || <false/> 不注     参数说明:    Nvidia独显 般都免驱,部分显卡打开Inject即可驱动成功。 2.如果遇到引导安装 屏,请尝试打开此注 。 3.部分显卡不开启这个注 ,即可直接驱动。请视情况 定,可能会跟 NVCAP 等参数组合使 来完善驱 动。
+ VRAM
  设置显存参数设置:该值为integer,单位为MB。 参数说明:正常来讲,Clover会 动注 正确的显存容量,当然多设置 下正确的显存也没有什么影响和风 险。如果显存识别错误, 如某些N卡,明明是1GB会被识别成1.5GB,请尝试使 此参数
+ DualLink
+ ig-platform-id
  0为开启 | 1为不开启参数说明: 1.此参数默认是1,也就是说即使Config 没有这个DualLink,Clover也会默认以1来设置。 2.此参数多 于解决 分屏开机屏幕被分成4 块的问题,如果有上述情况,请设置为0 3.此参数也可能解决 分屏引导 边/花屏等现象
### KernelAndKextPatches
+ AsusAICPUPM
  参数设定:<true/> || <false/>参数说明:  些供应商 如华硕,通常限制使 电源管理寄存器 的0xE2部分( 般是设置该部分为只读)。 在运  在2代SNB,3代IVB等平台的系统中,Mac的电源管理驱动会尝试向0xE2部分写 数据, 因为只读的限制会 导致内核崩溃(即因电源管理 五国),这个参数开启后,会限制电源管理驱动向0xE2写 的操作来避免五 国。+ KernelPm
  1.因Haswell平台锁定了MSR,开启此参数可避免 线重启。 2.在10.8.5/10.9.x的内核上测试通过
  
+ AppleRTC
  参数设定:<true/> || <false/>参数说明:防CMOS重置补丁,也可以使 DSDT修改Device (RTC)部分来解决CMOS重置问题
+ KernelLapic
  参数说明:解决HP笔记本的 Kernel Lapic 问题,或者也可以在启动参数上加 “cpus=1”来解决。
+ KextsToPatch
  **代码结构讲解:每 组patch都可以看成 个数组 参数说明:即可以给驱动内的Info.plist打补丁 也可以给 进制 件打补丁推荐系统稳定后,可以在Mac下 Clover Configurator来设置编辑 这部分请看 媛懒如此的Clover置顶帖  的 “任意kext打补丁(KextsToPatch)”**
### RtVariables
+ MountEFI
  参数名称:开机是否挂载EFI分区 参数设置:<true/> 开机挂载 || <false/> 开机不挂载

+ LogEveryBoot
  参数设置:Yes/No 参数说明:开机记录启动 志,默认开启
+ LogLineCount
  参数设置:阿拉伯数字,单位: (Line) 参数说明:设置最  志 数,避免每次都写  志导致log 件过 。
 
### SMBIOS

### GUI

     

  
    