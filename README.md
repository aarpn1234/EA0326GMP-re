# MT7981 U-Boot FIP 自动构建与 Release

本项目用于自动化构建 [MT7981 平台](https://source.denx.de/u-boot/u-boot) 的 **U-Boot** 与 **FIP 镜像**，并推送到 GitHub Release。

## 📦 功能特性
- 自动拉取最新 **Trusted Firmware-A** 与 **U-Boot** 源码
- 一键构建 BL31 (TF-A) 与 BL33 (U-Boot)
- 自动生成 **FIP 镜像**（带分区大小检测）
- SHA256 校验文件生成
- 自动创建或更新 GitHub Release 并上传产物

## 🚀 使用方法
1. Fork 或克隆本仓库到自己的账户
2. 检查 `.github/workflows/build-uboot.yml` 是否已存在
3. 在 GitHub 仓库 **Settings → Actions → General → Workflow permissions** 打开 **Read and write permissions**
4. 进入 **Actions** 标签页，手动运行 **Build MT7981 U-Boot FIP and Release**
   - 可自定义 Release tag 和标题
   - 可选择是否为 Pre-release

## 📂 构建产物
每次运行完成后，会在 Release 中生成：
- `fip.bin` — 主启动镜像
- `fip.bin.sha256` — FIP 镜像校验文件
- `u-boot.bin` — U-Boot BL33 镜像
- `u-boot.bin.sha256` — 校验文件

## 🛠 刷写指引
> ⚠ **风险提示**：刷写固件有风险，请先备份原厂固件。

```sh
# 备份原始分区
dd if=/dev/mtd1 of=/tmp/bl2_backup.bin
dd if=/dev/mtd4 of=/tmp/fip_backup.bin

# 上传新固件
scp fip.bin root@ROUTER:/tmp/

# 刷写并重启
mtd write /tmp/fip.bin FIP
sync && reboot
