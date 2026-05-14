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
