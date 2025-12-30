HP Zhan 66 G4 / ProBook 445 G8 AMD Hackintosh
![alt text](https://img.shields.io/badge/macOS-Ventura%2013.7.8-blue.svg)

![alt text](https://img.shields.io/badge/OpenCore-1.0.3-green.svg)

![alt text](https://img.shields.io/badge/CPU-AMD%20Cezanne-orange.svg)
一份针对 惠普战66四代锐龙版 (HP Zhan 66 G4 / ProBook 445 G8) 的 OpenCore 完美驱动配置文件。本配置解决了该机型在 512MB 锁定显存下的驱动死锁难题，实现了绝大部分硬件的完美日常使用。
💻 硬件规格
组件	详细型号	状态
CPU	AMD Ryzen 5 5600U (6C/12T, Cezanne)	🟢 完美 (1W-4W 待机)
显卡	AMD Radeon™ Vega 7 Graphics (512MB VRAM)	🟢 完美 (QE/CI 加速)
内存	16GB DDR4 3200MHz	🟢 正常
硬盘	NVMe SSD	🟢 正常
声卡	Realtek ALC236 (Layout ID: 13/3)	🟢 完美 (板载/耳机正常)
网卡	Intel Wi-Fi 6 AX200	🟢 完美 (原生 WiFi/802.1x)
触控板	I2C HID Device (Multi-touch)	🟢 完美 (多指手势)
显示屏	1080P IPS	🟢 完美 (支持 HiDPI)
摄像头	HP HD Camera (USB Internal)	🟢 正常
✨ 工作状态 (What's Working)

图形加速：通过 NootedRed 实现 QE/CI 加速，完美适配 512MB 锁定显存。

电源管理：支持 AMD P-State 动态调频，Package Power 待机功耗低至 1-4W。

亮度控制：支持系统亮度条调节及键盘快捷键。

声音输出：解决了 AMD 显卡与声卡的中断冲突（npci=0x2000）。

无线连接：AirportItlwm 驱动提供原生 WiFi 体验，支持校园网 802.1x。

输入设备：键盘、I2C 触控板手势完美工作。

USB 映射：全接口定制，摄像头与蓝牙内部端口识别正确。

显示缩放：已配置 HiDPI，字体清晰细腻。
🛠 配置要点 (Highlights)
1. 核心 Boot-Args (关键)
为了解决惠普 BIOS 锁定 512MB 显存导致的启动死锁，本配置采用了以下组合：
npci=0x2000 amdgpu_terrible_no_fb_vbl_irq=1 -nrednoip -nrednotool amdgpu_mod_ofb=1 -nrednoaudiopatch vmm-vga=1
npci=0x2000：解决 PCI 分配导致的声卡/显卡卡死。
amdgpu_mod_ofb=1：针对低显存机型的初始化优化。
-nrednoaudiopatch：解决 AMD APU 显卡驱动接管导致的音频总线锁死。
2. ACPI 补丁
SSDT-CPUR.aml：Ryzen 5000 移动端识别必需。
SSDT-XOSI.aml：解锁触控板与 GPIO 中断。
SSDT-PNLF-AMD.aml：开启 AMD 专用的背光控制。
SSDT-HPET.aml：修复 IRQ 中断冲突，找回声音。
⚠️ 避坑指南 (Tips)
浏览器崩溃：由于显存限制在 512MB，Google Chrome 等浏览器请务必在设置中关闭“硬件加速”，否则会导致系统死锁。
BIOS 设置：
Disable：Secure Boot, Fast Boot, IOMMU, Kernel DMA Protection (关键)。
Enable：UEFI Mode.
WiFi 认证：如果 AirportItlwm 无法自动弹出 802.1x 登录框，建议手动安装 .mobileconfig 描述文件强制连接。
三码信息：本 EFI 已移除 ROM, MLB, SystemSerialNumber, SystemUUID。上传前请务必使用 GenSMBIOS 生成你自己的三码，并确保网卡显示为 en0 且开启 Built-in。
🤝 鸣谢 (Credits)
Acidanthera 提供 OpenCore 及核心 Kexts。
ChefKissInc 提供 NootedRed 显卡驱动。
lzhoang2801 提供的 OpCore-Simplify 工具。
OpenIntelWireless 提供的 WiFi 驱动支持。
📄 免责声明 (Disclaimer)
本项目仅供学习研究使用。Hackintosh 存在一定风险，本人不对因使用此配置导致的任何硬件损坏或数据丢失负责。
