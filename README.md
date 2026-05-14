# huawei-ensp | [中文](README.zh-CN.md) |

## Introduction
Huawei eNSP (Enterprise Network Simulation Platform) is a network simulation tool designed for training and testing network configurations, particularly in enterprise environments. It allows users to create virtual network scenarios that replicate real-world configurations using Huawei's networking equipment. If you plan to take the HCIA/HCIP/HCIE certification exam, you can familiarize yourself with operating Huawei equipments (e.g., router, switch) on eNSP.

## Installation
After 2019, Huawei has paused sharing eNSP for strategic reasons. A "professional version" is only available to training institutions or device dealers. However, I have a backup of eNSP for individual users.

Before running the eNSP installation executable, you should gather the following pre-requisites:
- Oracle VM VirtualBox 5.2.44 (https://download.virtualbox.org/virtualbox/5.2.44/VirtualBox-5.2.44-139111-Win.exe)
- WinPcap 4.1.3 (https://www.winpcap.org/install/bin/WinPcap_4_1_3.exe)
- Wireshark 4.4.3 (https://2.na.dl.wireshark.org/win64/Wireshark-4.4.3-x64.exe)

Having installed all the pre-requisites, download the eNSP installation executable (542.2 MB, the file may be too big for a virus scan, download at your own discretion). Check the release on the right hand side of this page, you should find a place to download the executable there. (https://github.com/horserosemilkshake/huawei-ensp/releases/tag/eNSP)

The installation processes should be simple, just click "Next" all the way and it will work.

## Execution
The installed eNSP should launch normally at most cases, but starting a route device will (almost certainly) incur a 40 error.

![v2-261ce6a291c33cc34cf0080bf9ed04d0_1440w](https://github.com/user-attachments/assets/6bf28278-c160-4770-87ed-c829f52adc6c)

You can test whether you have this problem by clicking one of the examples in the main page (e.g., 1-1RIPv1&v2), drag and select all machine, and start them by clicking the "Start Device" button in the toolbar:

![image](https://github.com/user-attachments/assets/8cd8c8cb-324d-4ed0-aad6-ea4d99bbc0bd)

To solve a 40 error, follow the below steps:
- Disable Hyper-V (Turn Windows feature on and off > Hyper-V):
![image](https://github.com/user-attachments/assets/af197de3-7e75-4617-b5ec-bc4a737fd464)

- Disable memory integrity (Core isolation > Memory Integrity):
![image](https://github.com/user-attachments/assets/f9106704-b3e7-42d6-b108-7d9b1a978a21)

- Disable Hyper-V loading: Run `cmd` as an administrator. Run `bcdedit`. If `hypervisorlaunchtype` is `On` or `Auto`, turn it off with `bcdedit /set hypervisorlaunchtype off`.

- Reboot the machine and run eNSP as an administrator. The 40 error should be resolved.

## Windows 11 Crazy 40 Error Solution
If you still encounter the 40 error after using the above methods, Hyper-V cannot be completely disabled, and starting VirtualBox AR devices fails, use the following method:
- Download the dgreadiness tool
https://download.microsoft.com/download/b/d/8/bd821b1f-05f2-4a7e-aa03-df6c4f687b07/dgreadiness_v3.6.zip

After downloading, extract it, run PowerShell as administrator, and navigate to the extracted directory.

- Execute
```shell
Set-ExecutionPolicy Unrestricted -Scope CurrentUser
```
This command allows you to execute external scripts, otherwise the tool will report an error.

- Execute
```shell
.\DG_Readiness_Tool_v3.6.ps1 -Disable
```

- You should see output similar to the following
```output
./DG_Readiness_Tool_v3.6.ps1 -Disable
Security warning
Run only scripts that you trust. While scripts from the Internet can be useful, this script can potentially harm your computer. If you trust this script, use the Unblock-File cmdlet to allow the script to run without this warning message. Do you want to run E:\download\dgreeadiness\DG_Readiness_Tool_v3.6.ps1?
[D] Do not run(D)  [R] Run once(R)  [S] Suspend(S)  [?] Help (default is "D"): R
Directory: C:\Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         2026/5/14     15:30                DGLogs
###########################################################################
Readiness Tool Version 3.4 Release.
Tool to check if your device is capable to run Device Guard and Credential Guard.
###########################################################################
Disabling Device Guard and Credential Guard
Deleting RegKeys to disable DG/CG
Error: The system cannot find the specified registry key or value.
Error: The system cannot find the specified registry key or value.
del : Cannot find path "C:\WINDOWS\System32\CodeIntegrity\SIPolicy.p7b" because it does not exist.
At line:1 char:1
del  "$env:windir\System32\CodeIntegrity\SIPolicy.p7b"
  + CategoryInfo          : ObjectNotFound: (C:\WINDOWS\Syst...ty\SIPolicy.p7b:String) [Remove-Item], ItemNotFoundEx
 ception
  + FullyQualifiedErrorId : PathNotFound,Microsoft.PowerShell.Commands.RemoveItemCommand

Disabling Hyper-V and IOMMU
Disabling Hyper-V failed please check the log filePlease reboot the machine, for settings to be applied.
```

- Reboot

- After rebooting, before entering the system, the system will prompt whether to disable virtualization security, etc., there are 2 options, select disable for all.

- Then open VirtualBox, start an AR and try, if successful, say goodbye to the 40 error.

