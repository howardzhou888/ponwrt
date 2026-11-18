# XG040MD 固件编译配置

基于 zk-daepon (zk6685/zk-daepon) 定制的 XG040MD 光猫固件编译配置。

## 📋 新增功能

### 1. 科学上网
- ✅ **OpenClash** (vernesong/OpenClash dev分支) - 已包含中文语言包

### 2. 系统管理
- ✅ **定时重启** (luci-app-autoreboot) - 已包含中文语言包
- ✅ **ttyd Web终端** (luci-app-ttyd) - 已包含中文语言包

### 3. USB无线网卡驱动
支持以下芯片组：
- ✅ RTL8811CU (kmod-rtl8811cu)
- ✅ RTL88x2BU (kmod-rtl88x2bu) - 包含 8822BU/8812BU
- ✅ MT76x2U (kmod-mt76x2u) - 包含 MT7612U/MT7602U
- ✅ RTL8812AU (kmod-rtl8812au-ct)
- ✅ RTL8821CU (kmod-rtl8821cu)
- ✅ RTL8814AU (kmod-rtl8814au)
- ✅ RTL8723BU (kmod-rtl8723bu)
- ✅ RTL8192EU (kmod-rtl8192eu)
- ✅ MT7921AU (kmod-mt7921u)

### 4. NTP时间同步
预配置 NTP 服务器：
- `ntp.ntsc.ac.cn` (国家授时中心)
- `ntp1.aliyun.com` (阿里云)
- `cn.pool.ntp.org` (中国NTP池)
- `time.cloudflare.com` (Cloudflare)

### 5. 中文本地化
所有已安装插件均包含中文语言包 (luci-i18n-*-zh-cn)：
- Aurora主题配置、应用过滤、iStoreX、快速启动、应用商店
- 文件管理器、Samba4、WOL Plus、VLMCSD、DDNS
- ttyd、定时重启、Lucky、PON管理、NPU监控
- OpenClash、DDNS-Go、QModem

### 6. 自动化构建
- ✅ GitHub Actions 工作流自动编译
- ✅ 每天早上 5 点自动触发 (通过 Auto-Clean 工作流)
- ✅ 支持手动触发编译
- ✅ 输出 ITB 格式固件
- ✅ 自动发布到 GitHub Releases

## 🎯 目标设备

| 平台 | 设备 | 变体 |
|-----|------|------|
| **AN7581** | Nokia XG-040G-MD | ubi / ubi-usb-sfp |
| **AN7581** | Nokia XG-040G-TF | ubi |
| **AN7583** | Nokia XG-040G-MF | 普通版 / ubi |

## 🚀 使用方法

### 方案一：Fork 仓库自动编译 (推荐)

1. Fork [zk6685/zk-daepon](https://github.com/zk6685/zk-daepon) 仓库
2. 将本目录下的文件覆盖到 Fork 的仓库对应位置：
   ```
   Scripts/Packages.sh → Scripts/Packages.sh
   Scripts/Handles.sh → Scripts/Handles.sh
   Scripts/Settings.sh → Scripts/Settings.sh
   Config/GENERAL.txt → Config/GENERAL.txt
   Config/AIROHA-ONU.txt → Config/AIROHA-ONU.txt
   Config/AIROHA-ONU-AN7583.txt → Config/AIROHA-ONU-AN7583.txt
   .github/workflows/AIR-ONU.yml → .github/workflows/AIR-ONU.yml
   .github/workflows/WRT-CORE.yml → .github/workflows/WRT-CORE.yml
   ```
3. 推送到 GitHub，Actions 会自动开始编译
4. 编译完成后在 Releases 页面下载对应设备的 `.itb` 固件

### 方案二：本地编译

```bash
# 克隆源码
git clone https://github.com/zk6685/zk-daepon.git
cd zk-daepon

# 替换配置文件
cp -r /path/to/ponwrt-config/* .

# 手动触发编译 (需要 GitHub Actions 环境)
# 或使用 docker 本地编译 (较复杂，建议使用云编译)
```

### 方案三：使用现有 ponwrt 仓库

如果您有自己的 ponwrt 仓库 (https://github.com/howardzhou888/ponwrt)：
1. 将本配置合并到您的仓库
2. 修改 `.github/workflows/AIR-ONU.yml` 中的 `WRT_REPO` 指向您的源码
3. 推送触发编译

## 📦 固件命名规则

```
zk-daepon-<平台设备>-squashfs-sysupgrade-<源码>-<分支>-<日期>.itb
```

示例：
- `zk-daepon-airoha-an7581-nokia_xg-040g-md-ubi-squashfs-sysupgrade-pbs05-master-26.11.18-12.00.00.itb` (XG-040G-MD UBI版)
- `zk-daepon-airoha-an7581-nokia_xg-040g-md-ubi-usb-sfp-squashfs-sysupgrade-pbs05-master-26.11.18-12.00.00.itb` (XG-040G-MD USB-SFP版)

**请根据您的设备型号选择对应的 .itb 文件刷写！**

## ⚠️ 刷机前须知

1. **必须备份所有原厂分区**，特别是 `ri` 和 `bosa` 分区
2. 详细刷机教程：[恩山论坛](https://www.right.com.cn/forum/thread-8453612-1-1.html)
3. 刷机后如需恢复 MAC 地址，按教程刷回备份分区

## 🔧 默认配置

| 项目 | 值 |
|------|-----|
| 登录地址 | 192.168.1.1 |
| 登录密码 | 无 (首次登录建议设置) |
| 设备主机名 | daepon |
| WiFi 名称 | OWRT |
| WiFi 密码 | 12345678 |
| 默认主题 | Aurora |

## 📝 修改记录

- 基于 zk-daepon 仓库配置修改
- 新增 OpenClash、定时重启、ttyd
- 新增 9 种 USB 无线网卡驱动
- 配置 4 个国内 NTP 服务器
- 完善中文本地化支持
- 保留原有 PON、iStore、NPU 监控等功能

---

**源码来源**：https://github.com/zk6685/zk-daepon
**上游源码**：https://github.com/pbs05/ponwrt (master分支, kernel 6.18.55)
