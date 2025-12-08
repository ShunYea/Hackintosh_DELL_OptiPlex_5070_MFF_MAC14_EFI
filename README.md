# Hackintosh_DELL_OptiPlex_5070_MFF
Dell OptiPlex 5070 Hackintosh OpenCore EFI
- CPU用的是i9-9900T，网卡是很苹果的博通BCM943224PCIEBT2。
- 修改自github某位大佬的版本，也是忘记出处了，因为当时下载了很多，不知道是哪个版本能用的，就一直沿用下来。
- 功能基本是正常的，但是我本机WiFi是叹号，一直无法启用，查询了网上能查的所有方法，均无法启用WiFi，但是在不定时的重启后，会有几率WiFi可以使用，但是再重启又不行了。但是据原EFI出处，该博通网卡WiFi应该是正常的，是应该，但是我本机就是不正常。无解……稳定性很好，用了很久了，没有大的问题。
- 准备要想升级到更高的版本了，MACOS 26 Tahoe，尝尝鲜，也是黑苹果的最后一版了，虽然硬件性能可能不够，但是早升级早享受吧……作为14.8.1版本的EFI的存档，万一哪天需要回滚到这个老版本，就可以直接用了。PS：截止编辑此说明时，又有14.8.2了……

## 配置 ##
- CPU：i9-9900T
- 网卡：黑苹果博通BCM943224PCIEBT2
- MacOS：14.8.1
- OpenCore 1.0.6（1.0.4也是正常）

## BIOS设置 ##
这个很重要：
- General - Advanced Boot Options 取消勾选 Enable Legacy Option ROMs
- System Configuration - SATA Operation 选择 AHCI
- Video - Primary Display 选择 Intel HD Graphics
- Secure Boot - Secure Boot Enable 取消勾选 Secure Boot Enable
- Intel Software Guard Extensions - Intel SGX Enable 选择 Disabled
- Virtualization Support - Virtualization 勾选 Enable Intel Virtualization Technology
- Virtualization Support - VT for Direct I/O 取消勾选 Enable VT for Direct I/O
- Power Management - Block Sleep 勾选（非常重要，重置了BIOS后这个没设置，一直造成只要一睡眠就重启……）

使用grub工具进命令行设置：
- 关闭CFG Lock：setup_var 0x5BE 0x0
- 设置64M预分配显存：setup_var 0x8DC 0x2
