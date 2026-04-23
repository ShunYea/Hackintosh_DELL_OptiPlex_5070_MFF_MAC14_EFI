# 🍏 Hackintosh | DELL OptiPlex 5070 MFF EFI 存档

> **写在前面**：这是一份基于 macOS Sonoma 14.8.1 的 EFI 存档。考虑到后续计划升级到 macOS 26 Tahoe 尝鲜（这可能也是黑苹果时代的绝唱），特此留存这份 14.x 稳定版配置。万一后续新系统硬件吃不消需要回滚，可以直接取用。*(PS: 截止编辑此说明时，官方已推送 14.8.2)*

## 📝 项目与运行状态说明

* **EFI 来源**：基础版本修改自 GitHub 某位大佬的分享，历经多版本下载测试后筛选出此套配置，并一直沿用更新。
* **运行状态**：核心功能基本正常，长期使用稳定性极佳，没有出现过大的问题。
* ⚠️ **已知问题 (Wi-Fi 叹号)**：本机 Wi-Fi 无法正常启用（显示叹号），偶发重启后有几率可用，但再重启又失效，极不稳定。
  * *💡 避坑指南：原作者表示该博通网卡正常，但实际上苹果从 macOS Sonoma (14.0) 开始，在底层彻底删除了对大部分博通网卡的免驱支持。因此在 14.x 系统中出现叹号是系统机制导致的普遍现象，通常需要配合 OCLP (OpenCore Legacy Patcher) 强制打上 Root Patcher 补丁才能恢复。*

---

## ⚙️ 硬件与系统配置

| 核心部件 / 软件 | 型号 / 版本 | 备注说明 |
| :--- | :--- | :--- |
| **处理器 (CPU)** | Intel Core i9-9900T | |
| **无线网卡** | 博通 BCM943224PCIEBT2 | 曾是经典免驱卡（14.x 系统起需 OCLP 驱动） |
| **操作系统** | macOS Sonoma 14.8.1 | |
| **引导工具** | OpenCore 1.0.6 | *(经测试 1.0.4 版本亦可正常引导)* |

---

## 🔧 BIOS 核心设置

> ⚠️ **注意**：BIOS 设置对于黑苹果的成功引导至关重要，请仔细核对以下每一项。

| 菜单路径 | 设置项 | 目标状态 | 详细说明 |
| :--- | :--- | :--- | :--- |
| **General** | Advanced Boot Options | ❌ **取消勾选** | 禁用 *Enable Legacy Option ROMs* |
| **System Configuration**| SATA Operation | 💽 **AHCI** | 更改硬盘模式为 AHCI |
| **Video** | Primary Display | 🖥️ **Intel HD Graphics**| 强制使用 Intel 核显 |
| **Secure Boot** | Secure Boot Enable | ❌ **取消勾选** | 关闭安全启动 (*Secure Boot*) |
| **Intel SGX** | Intel SGX Enable | ❌ **Disabled** | 禁用 SGX 扩展 |
| **Virtualization Support**| Virtualization | ✅ **勾选** | 开启 *Intel Virtualization Technology* |
| **Virtualization Support**| VT for Direct I/O | ❌ **取消勾选** | 关闭 VT-d 功能 |
| **Power Management** | Block Sleep | ✅ **勾选** | 🚨 **极重要**：防止睡眠后自动重启（重置 BIOS 后极易漏设）|

---

## 💻 进阶设置 (GRUB 命令行)

请使用 GRUB 工具进入命令行环境，执行以下命令进行底层参数修改：

```bash
# 1. 关闭 CFG Lock (解锁 MSR 0xE2 寄存器)
setup_var 0x5BE 0x0

# 2. 设置 64M 预分配显存 (DVMT Pre-Allocated)
setup_var 0x8DC 0x2
