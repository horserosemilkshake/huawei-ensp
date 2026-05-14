# huawei-ensp

## 简介
Huawei eNSP（Enterprise Network Simulation Platform，企业网络模拟平台）是一款用于培训和测试网络配置的网络模拟工具，特别适用于企业环境。它允许用户创建虚拟网络场景，以模拟华为网络设备的真实配置。如果你计划参加 HCIA/HCIP/HCIE 认证考试，可以通过 eNSP 来熟悉华为设备（如路由器、交换机）的操作。

## 安装
2019 年之后，华为因战略原因停止公开分享 eNSP。“专业版”仅提供给培训机构或设备经销商。不过，我保存了适合个人用户使用的 eNSP 备份。
在运行 eNSP 安装程序之前，你需要提前安装以下前置条件：

Oracle VM VirtualBox 5.2.44
https://download.virtualbox.org/virtualbox/5.2.44/VirtualBox-5.2.44-139111-Win.exe

WinPcap 4.1.3
https://www.winpcap.org/install/bin/WinPcap_4_1_3.exe

Wireshark 4.4.3 
https://2.na.dl.wireshark.org/win64/Wireshark-4.4.3-x64.exe

安装好所有前置组件后，再下载 eNSP 安装程序（542.2 MB，文件可能过大无法进行病毒扫描，请自行斟酌）。在本页面右侧的 Release 中即可找到该安装文件：
https://github.com/horserosemilkshake/huawei-ensp/releases/tag/eNSP
安装过程很简单，一路点击“Next”即可完成。

## 运行
大多数情况下，安装后的 eNSP 可以正常启动，但在启动路由设备时（几乎肯定）会出现 40 错误。
![v2-261ce6a291c33cc34cf0080bf9ed04d0_1440w](https://github.com/user-attachments/assets/6bf28278-c160-4770-87ed-c829f52adc6c)

你可以在主界面中点击其中一个示例（如 1-1RIPv1&v2），拖拽选中所有设备，然后点击工具栏中的“Start Device”按钮测试是否出现此问题：
![image](https://github.com/user-attachments/assets/8cd8c8cb-324d-4ed0-aad6-ea4d99bbc0bd)

解决 40 错误的步骤如下：

关闭 Hyper-V（Windows 功能启用和关闭 > Hyper-V）：
![image](https://github.com/user-attachments/assets/af197de3-7e75-4617-b5ec-bc4a737fd464)

关闭内存完整性（Memory Integrity）（核心隔离 > 内存完整性）：
![image](https://github.com/user-attachments/assets/f9106704-b3e7-42d6-b108-7d9b1a978a21)

禁用 Hyper-V 加载：
以管理员身份运行 cmd，输入 bcdedit。
如果看到 hypervisorlaunchtype 为 On 或 Auto，使用以下命令关闭： ``bcdedit /set hypervisorlaunchtype off``

重启电脑并以管理员身份运行 eNSP。
这样应能解决 40 错误。

## windows 11 下疯狂40错误解决办法
如果你使用以上方式，仍然遇到40错误，hyper-v不能完全禁用，启动virtual box ar设备失败， 采用以下方法
- 下载dgreadiness工具
https://download.microsoft.com/download/b/d/8/bd821b1f-05f2-4a7e-aa03-df6c4f687b07/dgreadiness_v3.6.zip

下载后解压， powershell管理员身份运行然后进入到解压目录

- 执行
```shell
Set-ExecutionPolicy Unrestricted -Scope CurrentUser
```
此命令就是让你可以执行外部命令，不然工具执行会报错

- 执行
```shell
.\DG_Readiness_Tool_v3.6.ps1 -Disable
```

- 你大致看到如下输出
```output
./DG_Readiness_Tool_v3.6.ps1 -Disable安全警告
请只运行你信任的脚本。虽然来自 Internet 的脚本会有一定的用处，但此脚本可能会损坏你的计算机。如果你信任此脚本，请使用
Unblock-File cmdlet 允许运行该脚本，而不显示此警告消息。是否要运行 E:\download\dgreeadiness\DG_Readiness_Tool_v3.6.ps1?
[D] 不运行(D)  [R] 运行一次(R)  [S] 暂停(S)  [?] 帮助 (默认值为“D”): R目录: C:\Mode                 LastWriteTime         Length Name----                 -------------         ------ ----
d-----         2026/5/14     15:30                DGLogs
###########################################################################
Readiness Tool Version 3.4 Release.
Tool to check if your device is capable to run Device Guard and Credential Guard.
###########################################################################
Disabling Device Guard and Credential Guard
Deleting RegKeys to disable DG/CG
错误: 系统找不到指定的注册表项或值。
错误: 系统找不到指定的注册表项或值。
del : 找不到路径“C:\WINDOWS\System32\CodeIntegrity\SIPolicy.p7b”，因为该路径不存在。
所在位置 行:1 字符: 1del  "$env:windir\System32\CodeIntegrity\SIPolicy.p7b"

  + CategoryInfo          : ObjectNotFound: (C:\WINDOWS\Syst...ty\SIPolicy.p7b:String) [Remove-Item], ItemNotFoundEx
 ception
  + FullyQualifiedErrorId : PathNotFound,Microsoft.PowerShell.Commands.RemoveItemCommand

Disabling Hyper-V and IOMMU
Disabling Hyper-V failed please check the log filePlease reboot the machine, for settings to be applied.
```

- 重启

- 重启后进系统前系统会提示你要不要禁用虚拟化安全balabala， 共2个选项，全部选择禁用

- 然后点开virtual box，启动一个ar试试，成功即告别40错误。